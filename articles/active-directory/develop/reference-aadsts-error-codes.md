---
title: Codes d’erreur d’authentification et d’autorisation Azure AD
description: En savoir plus sur les codes d’erreur AADSTS retournés par le service d’émission de jeton de sécurité de Azure AD (STS).
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: reference
ms.date: 07/28/2021
ms.author: ryanwi
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: 8492b35fae2d2c2d716330002d5f889ec693f8c5
ms.sourcegitcommit: 87de14fe9fdee75ea64f30ebb516cf7edad0cf87
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/01/2021
ms.locfileid: "129353369"
---
# <a name="azure-ad-authentication-and-authorization-error-codes"></a>Codes d’erreur d’authentification et d’autorisation Azure AD

Vous souhaitez en savoir plus sur les codes d’erreur AADSTS retournés par le service d’émission de jeton de sécurité (STS) de Azure Active Directory (Azure AD) ? Lisez ce document pour rechercher les descriptions des erreurs AADSTS, leurs correctifs ainsi que d’autres solutions de contournement.

> [!NOTE]
> Ces informations sont provisoires et peuvent être modifiées. Vous avez une question ou vous ne trouvez pas ce que vous recherchez ? Créez un problème GitHub ou consultez [Options d’aide et de support pour les développeurs](./developer-support-help-options.md) pour en savoir plus sur les autres méthodes vous permettant d’obtenir de l’aide et un support.
>
> Cette documentation fournit des conseils aux développeurs et aux administrateurs ; elle ne doit jamais être utilisée par le client lui-même. Les codes d’erreur sont susceptibles d’être modifié à tout moment afin de fournir des messages d’erreur plus granulaires destinés à aider les développeurs lors de la création de leur application. Les applications qui dépendent des numéros de code d’erreur ou de texte seront endommagées au fil du temps.

## <a name="handling-error-codes-in-your-application"></a>Gestion des codes d’erreur dans votre application

La [spécification OAuth 2.0](https://tools.ietf.org/html/rfc6749#section-5.2) fournit des conseils sur la façon de gérer les erreurs d’authentification en utilisant la portion `error` de la réponse d’erreur. 

Voici un exemple de réponse d’erreur :

```json
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://example.contoso.com/activity.read is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
  "error_codes": [
    70011
  ],
  "timestamp": "2016-01-09 02:02:12Z",
  "trace_id": "255d1aef-8c98-452f-ac51-23d051240864",
  "correlation_id": "fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7", 
  "error_uri":"https://login.microsoftonline.com/error?code=70011"
}
```

| Paramètre         | Description    |
|-------------------|----------------|
| `error`       | Chaîne de code d’erreur utilisable pour classer les types d’erreurs se produisant, et à utiliser pour traiter les erreurs. |
| `error_description` | Un message d’erreur spécifique qui peut aider un développeur à identifier la cause principale d’une erreur d’authentification. N’utilisez jamais ce champ pour traiter une erreur dans votre code. |
| `error_codes` | Liste des codes d’erreur STS spécifiques pouvant être utiles dans les tests de diagnostic.  |
| `timestamp`   | Heure à laquelle l’erreur s’est produite. |
| `trace_id`    | Identifiant unique de la demande pouvant être utile dans les tests de diagnostic. |
| `correlation_id` | Identifiant unique de la demande pouvant être utile dans les tests de diagnostic sur les divers composants. |
| `error_uri` |  Lien vers la page de recherche d’erreur avec des informations supplémentaires sur l’erreur.  Il est réservé à l’usage des développeurs. Ne l’affichez pas pour les utilisateurs.  Présent uniquement lorsque le système de recherche d’erreur contient des informations supplémentaires sur l’erreur. Des informations supplémentaires ne sont pas disponibles pour toutes les erreurs.|

Le champ `error` a plusieurs valeurs possibles : consultez les liens de documentation sur le protocole et les spécifications OAuth 2.0 pour en savoir plus sur certaines erreurs spécifiques (par exemple, `authorization_pending` dans le [flux de code de l’appareil](v2-oauth2-device-code.md)) et savoir comment les traiter.  Certaines erreurs courantes sont répertoriées ici :

| Code d'erreur         | Description        | Action du client    |
|--------------------|--------------------|------------------|
| `invalid_request`  | Erreur de protocole, tel qu’un paramètre obligatoire manquant. | Corrigez l’erreur, puis envoyez à nouveau la demande.|
| `invalid_grant`    | Le matériel d’authentification (code d’authentification, jeton d’actualisation, jeton d’accès, vérification PKCE (Proof Key for Code Exchange)) était en partie non valide, non analysable, manquant ou inutilisable. | Essayez d’envoyer une nouvelle requête au point de terminaison `/authorize` pour obtenir un nouveau code d’autorisation.  Songez à examiner et valider l’utilisation des protocoles par cette application. |
| `unauthorized_client` | Le client authentifié n’est pas autorisé à utiliser ce type d’octroi d’autorisation. | Cela se produit généralement lorsque l’application cliente n’est pas inscrite dans Azure AD ou n’est pas ajoutée au client Azure AD de l’utilisateur. L’application peut proposer à l’utilisateur des instructions pour installer l’application et l’ajouter à Azure AD. |
| `invalid_client` | Échec d’authentification du client.  | Les informations d’identification du client ne sont pas valides. Pour résoudre le problème, l’administrateur de l’application met à jour les informations d’identification.   |
| `unsupported_grant_type` | Le serveur d’autorisation ne prend pas en charge le type d’octroi d’autorisation. | Modifiez le type d’octroi dans la demande. Ce type d’erreur doit se produire uniquement lors du développement et doit être détecté lors du test initial. |
| `invalid_resource` | La ressource cible n’est pas valide car elle n’existe pas, Azure AD ne la trouve pas ou elle n’est pas configurée correctement. | Cela indique que la ressource, si elle existe, n’a pas été configurée dans le client. L’application peut proposer à l’utilisateur des instructions pour installer l’application et l’ajouter à Azure AD.  Dans le cadre du développement, cela indique généralement qu’un locataire de test est mal configuré ou que le nom de l’étendue demandée contient une faute de frappe. |
| `interaction_required` | La demande nécessite une interaction utilisateur. Par exemple, une étape d’authentification supplémentaire est nécessaire. | Relancez la demande avec la même ressource, mais de manière interactive, afin que l’utilisateur puisse effectuer les vérifications demandées.  |
| `temporarily_unavailable` | Le serveur est temporairement trop occupé pour traiter la demande. | Relancez la requête. L’application cliente peut expliquer à l’utilisateur que sa réponse est reportée en raison d’une condition temporaire. |

## <a name="lookup-current-error-code-information"></a>Rechercher les informations actuelles sur les codes d’erreur
Les codes d’erreur et les messages sont susceptibles d’être modifiés.  Pour obtenir les informations les plus récentes, consultez la page [https://login.microsoftonline.com/error](https://login.microsoftonline.com/error) pour trouver les descriptions des erreurs AADSTS, des correctifs et des solutions suggérées.  

Par exemple, si vous avez reçu le code d’erreur « AADSTS50058 », effectuez une recherche dans [https://login.microsoftonline.com/error](https://login.microsoftonline.com/error) sur « 50058 ».  Vous pouvez également établir un lien direct à une erreur spécifique en ajoutant le numéro de code d’erreur à l’URL : [https://login.microsoftonline.com/error?code=50058](https://login.microsoftonline.com/error?code=50058).

## <a name="aadsts-error-codes"></a>Codes d’erreur AADSTS

| Error | Description |
|---|---|
| AADSTS16000 | SelectUserAccount : il s’agit d’une interruption levée par Azure AD, se traduisant par une interface utilisateur qui permet de choisir entre plusieurs sessions SSO valides. Cette erreur est assez courante et peut être retournée à l’application si `prompt=none` est spécifié. |
| AADSTS16001 | UserAccountSelectionInvalid : vous verrez cette erreur si l’utilisateur clique sur une vignette rejetée par la logique de sélection de session. Lorsque cette erreur est déclenchée, elle permet à l’utilisateur de récupérer en faisant un choix à partir d’une liste à jour des vignettes/sessions, ou en choisissant un autre compte. Cette erreur peut se produire en raison d’un défaut du code ou d’une condition de concurrence. |
| AADSTS16002 | AppSessionSelectionInvalid : l’exigence de SID spécifiée par l’application n’a pas été remplie.  |
| AADSTS16003 | SsoUserAccountNotFoundInResourceTenant : indique que l’utilisateur n’a pas été explicitement ajouté au locataire. |
| AADSTS17003 | CredentialKeyProvisioningFailed : Azure AD ne peut pas provisionner la clé de l’utilisateur. |
| AADSTS20001 | WsFedSignInResponseError : il existe un problème avec votre fournisseur d’identité fédérée. Contactez votre IDP pour résoudre ce problème. |
| AADSTS20012 | WsFedMessageInvalid : il existe un problème avec votre fournisseur d’identité fédérée. Contactez votre IDP pour résoudre ce problème. |
| AADSTS20033 | FedMetadataInvalidTenantName : il existe un problème avec votre fournisseur d’identité fédérée. Contactez votre IDP pour résoudre ce problème. |
| AADSTS28002 | La valeur fournie pour l’étendue du paramètre d’entrée « {scope} » n’est pas valide lors de la demande d’un jeton d’accès. Spécifiez un quota valide. |
| AADSTS28003 | La valeur fournie pour la portée du paramètre d’entrée ne peut pas être vide lors de la demande d’un jeton d’accès à l’aide du code d’autorisation fourni. Spécifiez un quota valide.|
| AADSTS40008 | OAuth2IdPUnretryableServerError : il existe un problème avec votre fournisseur d’identité fédérée. Contactez votre IDP pour résoudre ce problème. |
| AADSTS40009 | OAuth2IdPRefreshTokenRedemptionUserError : il existe un problème avec votre fournisseur d’identité fédérée. Contactez votre IDP pour résoudre ce problème. |
| AADSTS40010 | OAuth2IdPRetryableServerError : il existe un problème avec votre fournisseur d’identité fédérée. Contactez votre IDP pour résoudre ce problème. |
| AADSTS40015 | OAuth2IdPAuthCodeRedemptionUserError : il existe un problème avec votre fournisseur d’identité fédérée. Contactez votre IDP pour résoudre ce problème. |
| AADSTS50000 | TokenIssuanceError : il existe un problème avec le service d’inscription. [Ouvrez un ticket de support](../fundamentals/active-directory-troubleshooting-support-howto.md) pour résoudre ce problème. |
| AADSTS50001 | InvalidResource : la ressource est désactivée ou n’existe pas. Vérifiez le code de votre application pour vous assurer que vous avez spécifié l’URL de ressource exacte pour la ressource à laquelle vous essayez d’accéder.  |
| AADSTS50002 | NotAllowedTenant : la connexion a échoué en raison d’un accès proxy restreint sur le locataire. Si s’agit de votre propre stratégie de locataire, vous pouvez modifier les paramètres de locataire restreints pour résoudre ce problème. |
| AADSTS500021 | L’accès au locataire « {tenant} » est refusé. AADSTS500021 indique que la fonctionnalité de restriction du locataire est configurée et que l’utilisateur tente d’accéder à un locataire qui ne figure pas dans la liste des locataires autorisés spécifiés dans l’en-tête `Restrict-Access-To-Tenant`. Pour plus d’informations, consultez [Utiliser des restrictions liées au locataire pour gérer l’accès aux applications cloud SaaS](../manage-apps/tenant-restrictions.md).|
| AADSTS50003 | MissingSigningKey : la connexion a échoué en raison d’une clé de signature ou d’un certificat manquant. Il n’existe peut-être aucune clé de signature configurée dans l’application. Pour en savoir plus, consultez l’article sur la résolution des problèmes liés à l’erreur [AADSTS50003](/troubleshoot/azure/active-directory/error-code-aadsts50003-cert-or-key-not-configured). Si vous rencontrez toujours des problèmes, contactez le propriétaire ou l’administrateur de l’application. |
| AADSTS50005 | DevicePolicyError : l’utilisateur a essayé de se connecter à un appareil à partir d’une plateforme qui n’est actuellement pas prise en charge via la stratégie d’accès conditionnel. |
| AADSTS50006 | InvalidSignature : la vérification de la signature a échoué en raison d’une signature non valide. |
| AADSTS50007 | PartnerEncryptionCertificateMissing : le certificat de chiffrement partenaire est introuvable pour cette application. [Ouvrez un ticket de support](../fundamentals/active-directory-troubleshooting-support-howto.md) auprès de Microsoft pour résoudre ce problème. |
| AADSTS50008 | InvalidSamlToken : l’assertion SAML est manquante ou configurée de façon incorrecte dans le jeton. Contactez votre fournisseur de fédération. |
| AADSTS50010 | AudienceUriValidationFailed : la validation de l’URI d’audience pour l’application a échoué, car aucune audience de jeton n’a été configurée. |
| AADSTS50011 | InvalidReplyTo : l’adresse de réponse est manquante, configurée de façon incorrecte ou elle ne correspond pas aux adresses de réponse configurées pour l’application.  Pour la résolution, assurez-vous d’ajouter cette adresse de réponse manquante à l’application Azure Active Directory ou demandez à quelqu’un qui dispose des autorisations nécessaires pour gérer votre application dans Active Directory de le faire votre place. Pour en savoir plus, consultez l’article sur la résolution des problèmes liés à l’erreur [AADSTS50011](/troubleshoot/azure/active-directory/error-code-aadsts50011-reply-url-mismatch).|
| AADSTS50012 | AuthenticationFailed : échec de l’authentification pour l’une des raisons suivantes :<ul><li>Le nom du sujet du certificat de signature n’est pas autorisé</li><li>Aucune stratégie d’une autorité de confiance correspondante n’est trouvable pour le nom du sujet autorisé</li><li>La chaîne de certificats n'est pas valide</li><li>Le certificat de signature n’est pas valide</li><li>La stratégie n’est pas configurée sur le locataire</li><li>L’empreinte du certificat de signature n’est pas autorisée</li><li>L’assertion du client contient une signature non valide</li></ul> |
| AADSTS50013 | InvalidAssertion : l’assertion n’est pas valide pour différentes raisons : l’émetteur du jeton ne correspond pas à la version d’API dans l’intervalle de temps valide (expiré, format incorrect). Le jeton d’actualisation dans l’assertion n’est pas un jeton d’actualisation principal. |
| AADSTS50014 | GuestUserInPendingState : le remboursement de l’utilisateur est en attente. Le compte d’utilisateur invité n’a pas encore été entièrement créé. |
| AADSTS50015 | ViralUserLegalAgeConsentRequiredState : l’utilisateur exige le consentement légal d’une tranche d’âge. |
| AADSTS50017 | CertificateValidationFailed : la validation de la certification a échoué pour les raisons suivantes :<ul><li>Le certificat d’émission est introuvable dans la liste de certificats approuvés.</li><li>L’élément CrlSegment attendu est introuvable.</li><li>Le certificat d’émission est introuvable dans la liste de certificats approuvés.</li><li>Le point de distribution CRL delta est configuré sans point de distribution CRL correspondant.</li><li>Impossible de récupérer les segments CRL valides en raison d’un problème de délai d’expiration</li><li>Impossible de télécharger la liste CRL.</li></ul>Contactez l’administrateur du locataire. |
| AADSTS50020 | UserUnauthorized : les utilisateurs ne sont pas autorisés à appeler ce point de terminaison. |
| AADSTS50027 | InvalidJwtToken : jeton JWT non valide pour l’une des raisons suivantes :<ul><li>Aucune (sous-)revendication nonce</li><li>Non-concordance de l’identificateur du sujet</li><li>Revendication en double pour idToken</li><li>Émetteur inattendu</li><li>Audience inattendue</li><li>Intervalle de temps non valide </li><li>format de jeton incorrect</li><li>Échec du jeton d’ID externe de l’émetteur lors de la vérification de la signature.</li></ul> |
| AADSTS50029 | URI non valide. Le nom du domaine contient des caractères non valides. Contactez l’administrateur du locataire. |
| AADSTS50032 | WeakRsaKey : indique la tentative erronée de l’utilisateur pour utiliser une clé RSA faible. |
| AADSTS50033 | RetryableError : indique une erreur temporaire qui n’est pas liée aux opérations de base de données. |
| AADSTS50034 | UserAccountNotFound : pour se connecter à cette application, le compte doit être ajouté au répertoire. |
| AADSTS50042 | UnableToGeneratePairwiseIdentifierWithMissingSalt : la valeur salt nécessaire pour générer un identificateur par paire est manquante dans le principe. Contactez l’administrateur du locataire. |
| AADSTS50043 | UnableToGeneratePairwiseIdentifierWithMultipleSalts |
| AADSTS50048 | SubjectMismatchesIssuer : l’objet ne correspond pas à la revendication d’émetteur dans l’assertion du client. Contactez l’administrateur du locataire. |
| AADSTS50049 | NoSuchInstanceForDiscovery : instance inconnue ou non valide. |
| AADSTS50050 | MalformedDiscoveryRequest : le format de la requête est incorrect. |
| AADSTS50053 | Cette erreur peut résulter de deux raisons différentes : <br><ul><li>IdsLocked : le compte est verrouillé, car l’utilisateur a essayé de se connecter un trop grand nombre de fois avec un ID d’utilisateur ou un mot de passe incorrects. L’utilisateur est bloqué en raison de tentatives de connexion répétées. Voir [Atténuer les risques et débloquer les utilisateurs](../identity-protection/howto-identity-protection-remediate-unblock.md) ; ou</li><li>La connexion a été bloquée, car elle provient d’une adresse IP présentant des activités malveillantes.</li></ul> <br>Pour déterminer la raison de l’échec à l’origine de cette erreur, connectez-vous au [portail Azure](https://portal.azure.com).  Accédez à votre locataire Azure AD, puis à **Surveillance** -> **Connexions**. Recherchez la connexion d’utilisateur ayant échoué avec le **code d’erreur de connexion** 50053 et vérifiez la **raison de l’échec**.|
| AADSTS50055 | InvalidPasswordExpiredPassword : le mot de passe a expiré. Le mot de passe de l’utilisateur a expiré et, par conséquent, sa connexion ou sa session a été interrompue. Il lui sera proposé de le réinitialiser ou il pourra demander à un administrateur de le réinitialiser via [Réinitialiser le mot de passe d’un utilisateur à l’aide d’Azure Active Directory](../fundamentals/active-directory-users-reset-password-azure-portal.md). |
| AADSTS50056 | Mot de passe est nul ou non valide : le mot de passe pour cet utilisateur n’existe pas dans le répertoire. Il faut demander à l’utilisateur d’entrer à nouveau son mot de passe. |
| AADSTS50057 | UserDisabled : le compte d’utilisateur est désactivé. L’objet utilisateur dans Active Directory qui soutient ce compte a été désactivé. Un administrateur peut réactiver ce compte [via Powershell](/powershell/module/activedirectory/enable-adaccount). |
| AADSTS50058 | UserInformationNotProvided : les informations de session ne sont pas suffisantes pour l’authentification unique. Cela signifie qu’un utilisateur n’est pas connecté. Il s’agit d’une erreur courante qui est attendue lorsqu’un utilisateur n’est pas authentifié et n’est pas encore connecté.</br>Si cette erreur est rencontrée dans un contexte d'authentification unique où l'utilisateur s'est précédemment connecté, cela signifie que la session d'authentification unique est introuvable ou invalide.</br>Cette erreur peut être retournée à l’application si prompt=none est spécifié. |
| AADSTS50059 | MissingTenantRealmAndNoUserInformationProvided : les informations d’identification de locataire sont introuvables dans la requête ou déduites des informations d’identification fournies. L’utilisateur peut contacter l’administrateur du locataire pour qu’il l’aide à résoudre le problème. |
| AADSTS50061 | SignoutInvalidRequest : impossible de terminer la déconnexion. La demande n’était pas valide. |
| AADSTS50064 | CredentialAuthenticationError : la validation des informations d’identification sur le nom d’utilisateur ou le mot de passe a échoué. |
| AADSTS50068 | SignoutInitiatorNotParticipant : la déconnexion a échoué. L’application qui a initié la déconnexion ne participe pas à la session active. |
| AADSTS50070 | SignoutUnknownSessionIdentifier : la déconnexion a échoué. La demande de déconnexion a spécifié un identificateur de nom qui ne correspond à aucune session existante. |
| AADSTS50071 | SignoutMessageExpired : la demande de déconnexion a expiré. |
| AADSTS50072 | UserStrongAuthEnrollmentRequiredInterrupt : l’utilisateur doit s’inscrire pour l’authentification de second facteur (interactif). |
| AADSTS50074 | UserStrongAuthClientAuthNRequiredInterrupt : une authentification renforcée est nécessaire et l’utilisateur n’a pas réussi la vérification de l’authentification multifacteur. |
| AADSTS50076 | UserStrongAuthClientAuthNRequired : en raison d’une modification de la configuration apportée par l’administrateur, ou du déplacement vers un nouvel emplacement, l’utilisateur est tenu d’utiliser l’authentification multifacteur pour accéder à la ressource. Réessayez avec une nouvelle requête d’autorisation pour la ressource. |
| AADSTS50079 | UserStrongAuthEnrollmentRequired : en raison d’une modification de la configuration apportée par l’administrateur, ou du déplacement de l’utilisateur vers un nouvel emplacement, l’utilisateur est tenu d’utiliser l’authentification multifacteur. |
| AADSTS50085 | Le jeton d’actualisation a besoin d’une connexion IDP sociale. Demandez à l’utilisateur d’essayer à nouveau de se connecter avec son nom d’utilisateur et son mot de passe. |
| AADSTS50086 | SasNonRetryableError |
| AADSTS50087 | SasRetryableError : une erreur temporaire s’est produite lors de l’authentification forte. Recommencez. |
| AADSTS50088 | La limite des appels d’authentification multifacteur est atteinte. Réessayez dans quelques minutes. |
| AADSTS50089 | L’authentification a échoué en raison de l’expiration du jeton de flux. Attendu : les codes d’authentification, les jetons d’actualisation et les sessions expirent avec le temps ou sont révoqués par l’utilisateur ou un administrateur. L’application demandera une nouvelle connexion à l’utilisateur. |
| AADSTS50097 | DeviceAuthenticationRequired : l’authentification de l’appareil est requise. |
| AADSTS50099 | PKeyAuthInvalidJwtUnauthorized : la signature JWT n’est pas valide. |
| AADSTS50105 | EntitlementGrantsNotFound : l’utilisateur connecté n’est pas affecté à un rôle pour l’application concernée. Affectez l’utilisateur à l’application. Pour en savoir plus, consultez l’article sur la résolution des problèmes liés à l’erreur [AADSTS50105](/troubleshoot/azure/active-directory/error-code-aadsts50105-user-not-assigned-role). |
| AADSTS50107 | InvalidRealmUri : l’objet de domaine de fédération requis n’existe pas. Contactez l’administrateur du locataire. |
| AADSTS50120 | ThresholdJwtInvalidJwtFormat : problème avec l’en-tête JWT. Contactez l’administrateur du locataire. |
| AADSTS50124 | ClaimsTransformationInvalidInputParameter : la transformation des revendications contient un paramètre d’entrée non valide. Contactez l’administrateur du locataire pour mettre à jour la stratégie. |
| AADSTS501241 | L’entrée obligatoire « {paramName} » manque dans l’ID de transformation « {transformId} ». Cette erreur est renvoyée lorsqu’Azure AD tente de générer une réponse SAML à l’application. La revendication NameID ou NameIdentifier est obligatoire dans la réponse SAML et si Azure AD ne parvient pas à obtenir l’attribut source pour la revendication NameID, il renvoie cette erreur. Pour résoudre ce problème, assurez-vous d’ajouter des règles de revendication dans Portail Azure > Azure Active Directory > Applications d’entreprise > Sélectionner votre application > Authentification unique > Attributs utilisateur et revendications > Identifiant utilisateur unique (ID de nom).  |
| AADSTS50125 | PasswordResetRegistrationRequiredInterrupt : la connexion a été interrompue en raison d’une réinitialisation de mot de passe ou d’une entrée d’inscription de mot de passe. |
| AADSTS50126 | InvalidUserNameOrPassword : erreur de validation des informations d’identification en raison d’un nom d’utilisateur ou d’un mot de passe non valide. |
| AADSTS50127 | BrokerAppNotInstalled : l’utilisateur a besoin d’installer une application de répartiteur pour accéder à ce contenu. |
| AADSTS50128 | Nom de domaine non valide. Informations d’identification de locataire introuvables dans la requête ou déduites des informations d’identification fournies. |
| AADSTS50129 | DeviceIsNotWorkplaceJoined : un rattachement à l’espace de travail est nécessaire pour inscrire l’appareil. |
| AADSTS50131 | ConditionalAccessFailed : indique diverses erreurs d’accès conditionnel, comme un état de périphérique Windows incorrect, une requête bloquée en raison d’une activité, d’une stratégie d’accès ou de décisions de stratégie de sécurité suspectes. |
| AADSTS50132 | SsoArtifactInvalidOrExpired : la session n’est pas valide en raison de l’expiration du mot de passe ou de sa modification récente. |
| AADSTS50133 | SsoArtifactRevoked : la session n’est pas valide en raison de l’expiration du mot de passe ou de sa modification récente. |
| AADSTS50134 | DeviceFlowAuthorizeWrongDatacenter : centre de données incorrect. Pour autoriser une demande initiée par une application dans le flux d’appareil OAuth 2.0, la partie qui accorde l’autorisation doit figurer dans le même centre de données que la demande d’origine. |
| AADSTS50135 | PasswordChangeCompromisedPassword : La modification du mot de passe est nécessaire en raison du risque du compte. |
| AADSTS50136 | RedirectMsaSessionToApp : une seule session MSA est détectée. |
| AADSTS50139 | SessionMissingMsaOAuth2RefreshToken : la session n’est pas valide en raison d’un jeton d’actualisation externe manquant. |
| AADSTS50140 | KmsiInterrupt : cette erreur s’est produite suite à l’interruption de la fonction « Maintenir la connexion » lors de la connexion de l’utilisateur. Il s’agit d’une partie attendue du flux de connexion, où il est demandé à l’utilisateur s’il souhaite rester connecté à son navigateur actuel pour faciliter les connexions ultérieures. Pour plus d’informations, consultez [The new Azure AD sign-in and “Keep me signed in” experiences rolling out now!](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/the-new-azure-ad-sign-in-and-keep-me-signed-in-experiences/m-p/128267) (Nouvelles expériences de connexion Azure AD et de maintien de la connexion en cours de déploiement). Vous pouvez [ouvrir un ticket de support](../fundamentals/active-directory-troubleshooting-support-howto.md) avec l’ID de corrélation, l’ID de requête et le code d’erreur pour obtenir plus de détails.|
| AADSTS50143 | Incompatibilité de session. La session n’est pas valide, car le locataire de l’utilisateur ne correspond pas à l’indicateur de domaine en raison d’une ressource différente. [Ouvrez un ticket de support](../fundamentals/active-directory-troubleshooting-support-howto.md) avec l’ID de corrélation, l’ID de requête et le code d’erreur pour obtenir plus de détails. |
| AADSTS50144 | InvalidPasswordExpiredOnPremPassword : Le mot de passe Active Directory de l’utilisateur est arrivé à expiration. Générez un nouveau mot de passe pour l’utilisateur ou demandez à l’utilisateur d’utiliser l’outil de réinitialisation en libre-service pour le réinitialiser. |
| AADSTS50146 | MissingCustomSigningKey : cette application doit être configurée avec une clé de signature spécifique. Elle n’est configuré avec aucune clé, ou la clé a expiré ou n’est pas encore valide. |
| AADSTS50147 | MissingCodeChallenge : la taille du paramètre de vérification du code n’est pas valide. |
| AADSTS501481 | L’élément Code_Verifier ne correspond pas à l’élément code_challenge fourni dans la requête d’autorisation.|
| AADSTS50155 | DeviceAuthenticationFailed : l’authentification de l’appareil a échoué pour cet utilisateur. |
| AADSTS50158 | ExternalSecurityChallenge : la vérification de sécurité externe n’a pas abouti. |
| AADSTS50161 | InvalidExternalSecurityChallengeConfiguration : les revendications envoyées par un fournisseur externe ne sont pas suffisantes, ou il manque une revendication demandée à un fournisseur externe. |
| AADSTS50166 | ExternalClaimsProviderThrottled : l’envoi de la requête au fournisseur de revendications a échoué. |
| AADSTS50168 | ChromeBrowserSsoInterruptRequired : le client est capable d’obtenir un jeton d’authentification unique via l’extension des comptes Windows 10, mais le jeton est introuvable dans la requête ou bien le jeton fourni a expiré. |
| AADSTS50169 | InvalidRequestBadRealm : le domaine n’est pas un domaine configuré de l’espace de noms de service actuel. |
| AADSTS50170 | MissingExternalClaimsProviderMapping : le mappage des contrôles externes est manquant. |
| AADSTS50173 | FreshTokenNeeded : l’octroi fourni a expiré en raison de sa révocation et un nouveau jeton d’authentification est nécessaire. Un administrateur ou un utilisateur a révoqué les jetons pour cet utilisateur, ce qui entraîne l’échec des actualisations de jetons suivantes et nécessite une nouvelle authentification. L’utilisateur doit se connecter à nouveau. |
| AADSTS50177 | ExternalChallengeNotSupportedForPassthroughUsers : la vérification externe n’est pas prise en charge pour les utilisateurs de PassThrough. |
| AADSTS50178 | SessionControlNotSupportedForPassthroughUsers : le contrôle de session n’est pas pris en charge pour les utilisateurs de PassThrough. |
| AADSTS50180 | WindowsIntegratedAuthMissing : l’authentification Windows intégrée est nécessaire. Activez le locataire pour l’authentification unique transparente. |
| AADSTS50187 | DeviceInformationNotProvided : le service n’a pas réussi à authentifier l’appareil. |
| AADSTS50194 | L’application « {appId} » ({appName}) n’est pas configurée en tant qu’application mutualisée. L’utilisation du point de terminaison /common n’est pas prise en charge pour les applications créées après « {time} ». Utilisez un point de terminaison spécifique au locataire ou configurez l’application pour qu’elle soit mutualisée. |
| AADSTS50196 | LoopDetected : une boucle client a été détectée. Vérifiez la logique de l’application pour vous assurer que la mise en cache des jetons est implémentée et que les conditions d’erreur sont gérées correctement.  L’application a effectué un trop grand nombre de requêtes sur une période trop brève, indiquant qu’elle est dans un état défectueux ou qu’elle demande trop de jetons. |
| AADSTS50197 | ConflictingIdentities : impossible de trouver l’utilisateur. Réessayez de vous connecter. |
| AADSTS50199 | CmsiInterrupt : pour des raisons de sécurité, une confirmation de l’utilisateur est requise pour cette demande.  Étant donné qu’il s’agit d’une erreur « interaction_required », le client doit effectuer une authentification interactive.  Cela est dû au fait qu’un affichage web système a été utilisé pour demander un jeton pour une application native : l’utilisateur doit être invité à préciser s’il s’agissait effectivement de l’application à laquelle il voulait se connecter. Pour éviter cette invite, l’URI de redirection doit faire partie de la liste approuvée suivante : <br />http://<br />https://<br />msauth://(iOS uniquement)<br />msauthv2://(iOS uniquement)<br />chrome-extension:// (navigateur Chrome appareil de bureau uniquement) |
| AADSTS51000 | RequiredFeatureNotEnabled : la fonctionnalité est désactivée. |
| AADSTS51001 | DomainHintMustbePresent : l’indicateur de domaine doit être présent avec l’identificateur de sécurité local ou l’UPN local. |
| AADSTS51004 | UserAccountNotInDirectory : le compte d’utilisateur n’existe pas dans le répertoire. |
| AADSTS51005 | TemporaryRedirect : l’équivalent de l’état HTTP 307, qui indique que les informations demandées se trouvent au niveau de l’URI spécifié dans l’en-tête Location. Lorsque vous recevez cet état, suivez l’en-tête Location associé à la réponse. Lorsque la méthode de requête d’origine est POST, la requête redirigée l’utilise également. |
| AADSTS51006 | ForceReauthDueToInsufficientAuth : l’authentification Windows intégrée est nécessaire. L’utilisateur s’est connecté à l’aide d’un jeton de session ne contenant pas la revendication de l’authentification Windows intégrée. Demandez à l’utilisateur de se connecter à nouveau. |
| AADSTS52004 | DelegationDoesNotExistForLinkedIn : l’utilisateur n’a pas donné son consentement pour l’accès aux ressources de LinkedIn. |
| AADSTS53000 | DeviceNotCompliant : une stratégie d’accès conditionnel nécessite un appareil conforme, or l’appareil n’est pas conforme. L’utilisateur doit inscrire ses appareils auprès d’un fournisseur approuvé de gestion des périphériques mobiles, comme Intune. |
| AADSTS53001 | DeviceNotDomainJoined : une stratégie d’accès conditionnel nécessite un appareil de jonction de domaine, or l’appareil n’est pas joint au domaine. Demandez à l’utilisateur d’utiliser un appareil de jonction de domaine. |
| AADSTS53002 | ApplicationUsedIsNotAnApprovedApp : l’application utilisée n’est pas une application approuvée pour l’accès conditionnel. L’utilisateur doit utiliser une des options de la liste des applications approuvées afin d’obtenir l’accès. |
| AADSTS53003 | BlockedByConditionalAccess : l’accès a été bloqué par des stratégies d’accès conditionnel. La stratégie d’accès n’autorise pas l’émission de jeton. |
| AADSTS53004 | ProofUpBlockedDueToRisk : l’utilisateur doit terminer le processus d’inscription de l’authentification multifacteur pour accéder à ce contenu. L’utilisateur doit s’inscrire à l’authentification multifacteur. |
| AADSTS53011 | Utilisateur bloqué en raison d’un risque sur le locataire de base. |
| AADSTS54000 | MinorUserBlockedLegalAgeGroupRule |
| AADSTS54005 | Le code d’autorisation OAuth2 a déjà été utilisé, réessayez avec un nouveau code valide ou utilisez un jeton d’actualisation existant. |
| AADSTS65001 | DelegationDoesNotExist : l’utilisateur ou l’administrateur n’a pas accepté d’utiliser l’application avec ID X. Envoyez une requête d’autorisation interactive pour cet utilisateur et cette ressource. |
| AADSTS65002 | Le consentement entre l’application du premier tiers '{applicationId}' et la ressource de la première partie '{resourceId}'doit être configuré par le biais de la pré-autorisation. Les applications détenues et gérées par Microsoft doivent obtenir l’approbation du propriétaire de l’API avant toute demande de jetons pour cette API.  Un développeur de votre locataire peut tenter de réutiliser un ID d’application détenu par Microsoft. Cette erreur l’empêche d’usurper l’identité d’une application Microsoft pour appeler d’autres API. Ils doivent passer à une autre ID d’application qu’ils inscrivent dans https://portal.azure.com.|
| AADSTS65004 | UserDeclinedConsent : l’utilisateur a refusé de donner son consentement pour accéder à l’application. Demandez à l’utilisateur de réessayer de se connecter et de donner son consentement à l’application.|
| AADSTS65005 | MisconfiguredApplication : la liste d’accès aux ressources requise par l’application ne contient pas d’applications détectables par la ressource ; l’application cliente a demandé un accès à la ressource qui n’était pas spécifié dans sa liste d’accès aux ressources requise ; le service Graph a renvoyé une requête incorrecte ou la ressource est introuvable. Si l’application prend en charge SAML, vous avez peut-être configuré l’application avec un identificateur incorrect (entité). Pour en savoir plus, consultez l’article sur la résolution des problèmes liés à l’erreur [AADSTS650056](/troubleshoot/azure/active-directory/error-code-aadsts650056-misconfigured-app). |
| AADSTS650052 | L’application a besoin d’accéder à un service `(\"{name}\")` auquel votre organisation `\"{organization}\"` n’est pas abonnée ou qu’elle n’a pas activé. Contactez votre administrateur informatique pour examiner la configuration de vos abonnements de service. |
| AADSTS650054 |  L’application a demandé des autorisations pour accéder à une ressource qui a été supprimée ou qui n’est plus disponible. Assurez-vous que toutes les ressources que l’application appelle sont présentes dans le locataire dans lequel vous travaillez. |
| AADSTS650056 | Application mal configurée. La raison peut être l’une des suivantes : le client n’a listé aucune autorisation pour « {name} » parmi les autorisations demandées dans l’inscription d’application du client. Ou l’administrateur n’a pas donné son consentement dans le locataire. Vous pouvez aussi vérifier l’identificateur d’application dans la requête pour vous assurer qu’il correspond à l’identificateur d’application cliente configuré. Sinon, vérifiez le certificat dans la demande pour vous assurer qu’il est valide. Contactez votre administrateur pour corriger la configuration ou donner le consentement au nom du locataire. ID de l’application cliente : {id}. Contactez votre administrateur pour corriger la configuration ou donner le consentement au nom du locataire.|
| AADSTS650057 | ressource non valide. Le client a demandé l’accès à une ressource qui n’est pas listée dans les autorisations demandées dans l’inscription d’application du client. ID d’application cliente : {appId} ({appName}). Valeur de la ressource à partir de la demande : {resource}. ID de l’application de la ressource : {resourceAppId}. Liste des ressources valides de l’inscription de l’application : {regList}. |
| AADSTS67003 | ActorNotValidServiceIdentity |
| AADSTS70000 | InvalidGrant : échec d’authentification. Le jeton d’actualisation n'est pas valide. L’erreur peut être due à l’une des raisons suivantes :<ul><li>L’en-tête de liaison de jeton est vide</li><li>Le hachage de liaison de jeton ne correspond pas</li></ul> |
| AADSTS70001 | UnauthorizedClient : l’application est désactivée. Pour en savoir plus, consultez l’article sur la résolution des problèmes liés à l’erreur [AADSTS70001](/troubleshoot/azure/active-directory/error-code-aadsts70001-app-not-found-in-directory). |
| AADSTS70002 | InvalidClient : erreur de validation des informations d’identification. La clé secrète client (client_secret) spécifiée ne correspond pas à la valeur attendue pour ce client. Corrigez la clé secrète client, puis réessayez. Pour plus d’informations, consultez [Utiliser le code d’autorisation pour demander un jeton d’accès](v2-oauth2-auth-code-flow.md#redeem-a-code-for-an-access-token). |
| AADSTS70003 | UnsupportedGrantType : l’application a retourné un type d’autorisation non pris en charge. |
| AADSTS700030 | Certificat non valide : le nom d’objet du certificat n’est pas autorisé. Les SubjectNames/SubjectAlternativeNames (jusqu’à 10) dans le certificat de jeton sont : {certificateSubjects}. |
| AADSTS70004 | InvalidRedirectUri : l’application a retourné un URI de redirection non valide. L’adresse de redirection spécifiée par le client ne correspond à aucune adresse configurée ni à aucune adresse de la liste d’approbation OIDC. |
| AADSTS70005 | UnsupportedResponseType : l’application a renvoyé un type de réponse non pris en charge pour les raisons suivantes :<ul><li>Le type de réponse « jeton » n’est pas activé pour l’application</li><li>Le type de réponse « id_token » requiert la portée « OpenID ». La réponse contient une valeur de paramètre OAuth non prise en charge dans l’élément wctx codé.</li></ul> |
| AADSTS700054 | Response_type « id_token » n’est pas activé pour l’application.  L’application a demandé un jeton d’ID au point de terminaison d’autorisation, mais n’a pas activé l’octroi implicite de jeton d’ID.  Accédez à Portail Azure > Azure Active Directory > Inscriptions d’applications > Sélectionner votre application > Authentification > Sous « Octroi implicite et flux hybrides », assurez-vous que « Jetons d’ID » est sélectionné.|
| AADSTS70007 | UnsupportedResponseMode : l’application a renvoyé une valeur non prise en charge pour `response_mode` lors de la requête d’un jeton.  |
| AADSTS70008 | ExpiredOrRevokedGrant : le jeton d’actualisation a expiré en raison d’une inactivité. Le jeton a été émis le XXX et il était inactif pendant un certain laps de temps. |
| AADSTS700084 | Le jeton d’actualisation ayant été délivré à une application monopage (SPA), il a une durée de vie fixe et limitée de {time}, laquelle ne peut pas être prolongée. Il a entre-temps expiré et une nouvelle demande de connexion doit être envoyée par la SPA à la page de connexion. Le jeton a été délivré le {issueDate}.|
| AADSTS70011 | InvalidScope : la portée demandée par l’application n’est pas valide. |
| AADSTS70012 | MsaServerError : une erreur de serveur s’est produite lors de l’authentification d’un utilisateur de compte de service administré (consommateur). Réessayez. Si le problème persiste, [ouvrez un ticket de support](../fundamentals/active-directory-troubleshooting-support-howto.md). |
| AADSTS70016 | AuthorizationPending : erreur de flux d’appareil d’OAuth 2.0. Une autorisation est en attente. L’appareil va réessayer l’interrogation de la requête. |
| AADSTS70018 | BadVerificationCode : le code de vérification n’est pas valide, car l’utilisateur a saisi un code utilisateur incorrect pour le flux de code d’appareil. L’autorisation n’est pas approuvée. |
| AADSTS70019 | CodeExpired : le code de vérification a expiré. Demandez à l’utilisateur de réessayer de se connecter. |
| AADSTS70043 | Le jeton d’actualisation a expiré ou n’est pas valide en raison des vérifications de la fréquence de connexion par l’accès conditionnel. Le jeton a été émis le {issueDate} et la durée de vie maximale autorisée pour cette demande est {time}. |
| AADSTS75001 | BindingSerializationError : une erreur s’est produite lors de la liaison de message SAML. |
| AADSTS75003 | UnsupportedBindingError : l’application a renvoyé une erreur relative à une liaison non prise en charge (impossible d’envoyer une réponse de protocole SAML via des liaisons autres que HTTP POST). |
| AADSTS75005 | Saml2MessageInvalid : Azure AD ne prend pas en charge les requêtes SAML envoyées par l’application pour l’authentification unique. Pour en savoir plus, consultez l’article sur la résolution des problèmes liés à l’erreur [AADSTS75005](/troubleshoot/azure/active-directory/error-code-aadsts75005-not-a-valid-saml-request). |
| AADSTS7500514 | Impossible de trouver un type de réponse SAML pris en charge. Les types de réponse pris en charge sont « Response » (dans l’espace de noms XML « urn:oasis:names:tc:SAML:2.0:protocol ») ou « Assertion » (dans l’espace de noms XML « urn:oasis:names:tc:SAML:2.0:assertion »). Erreur de l’application : le développeur va gérer cette erreur.|
| AADSTS750054 | SAMLRequest ou SAMLResponse doit être présent en tant que paramètres de la chaîne de requête dans la requête HTTP pour la liaison de redirection SAML. Pour en savoir plus, consultez l’article sur la résolution des problèmes liés à l’erreur [AADSTS750054](/troubleshoot/azure/active-directory/error-code-aadsts750054-saml-request-not-present). |
| AADSTS75008 | RequestDeniedError : la requête provenant de l’application a été refusée, car la requête SAML avait une destination inattendue. |
| AADSTS75011 | NoMatchedAuthnContextInOutputClaims : la méthode d’authentification de l’utilisateur auprès du service ne correspond pas à la méthode d’authentification demandée. Pour en savoir plus, consultez l’article sur la résolution des problèmes liés à l’erreur [AADSTS75011](/troubleshoot/azure/active-directory/error-code-aadsts75011-auth-method-mismatch). |
| AADSTS75016 | Saml2AuthenticationRequestInvalidNameIDPolicy : dans la requête d’authentification SAML2, NameIdPolicy n’est pas valide. |
| AADSTS80001 | OnPremiseStoreIsNotAvailable : l’agent d’authentification ne peut pas se connecter à Active Directory. Assurez-vous que les serveurs des agents sont membres de la même forêt Active Directory que les utilisateurs dont les mots de passe doivent être validés, et qu’ils peuvent se connecter à Active Directory. |
| AADSTS80002 | OnPremisePasswordValidatorRequestTimedout : La requête de validation du mot de passe est arrivée à expiration. Vérifiez qu’Active Directory est disponible et répond aux requêtes des agents. |
| AADSTS80005 | OnPremisePasswordValidatorUnpredictableWebException : une erreur inconnue s’est produite lors du traitement de la réponse provenant de l’agent d’authentification. Relancez la requête. Si le problème persiste, [ouvrez un ticket de support](../fundamentals/active-directory-troubleshooting-support-howto.md) pour plus d’informations sur l’erreur. |
| AADSTS80007 | OnPremisePasswordValidatorErrorOccurredOnPrem : l’agent d’authentification n’est pas en mesure de valider le mot de passe de l’utilisateur. Consultez les journaux d’activité de l’agent pour plus d’informations, et vérifiez qu’Active Directory fonctionne comme prévu. |
| AADSTS80010 | OnPremisePasswordValidationEncryptionException : l’agent d’authentification n’est pas en mesure de déchiffrer le mot de passe. |
| AADSTS80012 | OnPremisePasswordValidationAccountLogonInvalidHours : les utilisateurs ont essayé de se connecter en dehors des heures autorisées (spécifiées dans Active Directory). |
| AADSTS80013 | OnPremisePasswordValidationTimeSkew : la tentative d’authentification n’a pas abouti en raison du temps de décalage entre l’ordinateur exécutant l’agent d’authentification et Active Directory. Résolvez les problèmes de synchronisation. |
| AADSTS81004 | DesktopSsoIdentityInTicketIsNotAuthenticated : Échec de la tentative d’authentification Kerberos. |
| AADSTS81005 | DesktopSsoAuthenticationPackageNotSupported : le package d’authentification n’est pas pris en charge. |
| AADSTS81006 | DesktopSsoNoAuthorizationHeader : Aucun en-tête d’autorisation n’a été trouvé. |
| AADSTS81007 | DesktopSsoTenantIsNotOptIn : Le locataire n’est pas activé pour l’authentification unique transparente. |
| AADSTS81009 | DesktopSsoAuthorizationHeaderValueWithBadFormat : Impossible de valider le ticket Kerberos de l’utilisateur. |
| AADSTS81010 | DesktopSsoAuthTokenInvalid : l’authentification unique transparente a échoué, car le ticket Kerberos de l’utilisateur est arrivé à expiration ou n’est pas valide. |
| AADSTS81011 | DesktopSsoLookupUserBySidFailed : impossible de trouver l’objet utilisateur à partir des informations contenues dans le ticket Kerberos de l’utilisateur. |
| AADSTS81012 | DesktopSsoMismatchBetweenTokenUpnAndChosenUpn : l’utilisateur qui tente de se connecter à Azure AD est différent de l’utilisateur connecté à l’appareil. |
| AADSTS90002 | InvalidTenantName : le nom du locataire n’a pas été trouvé dans le magasin de données. Assurez-vous d’avoir l’ID de locataire correct. |
| AADSTS90004 | InvalidRequestFormat : le format de la demande est incorrect. |
| AADSTS90005 | InvalidRequestWithMultipleRequirements : impossible de terminer la demande. La demande n’est pas valide car l’identificateur et l’indicateur de connexion ne peut pas être utilisés conjointement.  |
| AADSTS90006 | ExternalServerRetryableError : le service est temporairement indisponible.|
| AADSTS90007 | InvalidSessionId : demande incorrecte. L’ID de la session précédente ne peut pas être analysé. |
| AADSTS90008 | TokenForItselfRequiresGraphPermission : l’utilisateur ou l’administrateur n’a pas accepté d’utiliser l’application. Au minimum, l’application requiert l’accès à Azure AD en spécifiant l’autorisation de connexion et de lecture du profil utilisateur. |
| AADSTS90009 | TokenForItselfMissingIdenticalAppIdentifier : l’application demande un jeton pour elle-même. Ce scénario est pris en charge uniquement si la ressource spécifiée utilise l’ID d’application basé sur GUID. |
| AADSTS90010 | NotSupported : impossible de créer l’algorithme. |
| AADSTS9001023 |Le type d’autorisation n’est pas pris en charge sur les points de terminaison /common ou /consumers. Utilisez le point de terminaison /organizations ou propre au locataire.|
| AADSTS90012 | RequestTimeout : la demandé a expiré. |
| AADSTS90013 | InvalidUserInput : l’entrée de l’utilisateur n’est pas valide. |
| AADSTS90014 | MissingRequiredField : ce code d’erreur peut apparaître dans différents cas lorsqu’un champ attendu est absent des informations d’identification. |
| AADSTS900144 | Le corps de la requête doit contenir le paramètre : '{name}'.  Erreur du développeur : l’application tente de se connecter sans les paramètres d’authentification nécessaires ou corrects.|
| AADSTS90015 | QueryStringTooLong : la chaîne de la requête est trop longue. |
| AADSTS90016 | MissingRequiredClaim : le jeton d’accès n’est pas valide. La revendication requise est manquante. |
| AADSTS90019 | MissingTenantRealm : Azure AD n’a pas pu déterminer l’identificateur de locataire à partir de la requête. |
| AADSTS90020 | L’assertion SAML 1.1 ne contient pas l’ImmutableID de l’utilisateur. Erreur du développeur : l’application tente de se connecter sans les paramètres d’authentification nécessaires ou corrects.|
| AADSTS90022 | AuthenticatedInvalidPrincipalNameFormat : le format du nom du principal n’est pas valide ou ne répond pas au format `name[/host][@realm]` attendu. Le nom du principal est requis, l’hôte et le domaine sont facultatifs et peuvent être définis sur null. |
| AADSTS90023 | InvalidRequest - La demande de service d’authentification n’est pas valide. |
| AADSTS9002313 | InvalidRequest - La requête est non valide ou incorrecte. - Le problème ici est la conséquence d’une erreur survenue au niveau de la requête à un point de terminaison donné. La suggestion pour corriger ce problème consiste à obtenir une trace Fiddler de l’erreur qui se produit et à vérifier si la requête est correctement mise en forme ou non. |
| AADSTS90024 | RequestBudgetExceededError : une erreur temporaire s’est produite. Réessayez. |
| AADSTS90027 | Nous ne pouvons pas émettre de jetons à partir de cette version d’API sur le locataire MSA. Contactez le fournisseur de l’application, car il doit utiliser la version 2.0 du protocole pour la prendre en charge.|
| AADSTS90033 | MsodsServiceUnavailable : Microsoft Online Directory Service (MSODS) n’est pas disponible. |
| AADSTS90036 | MsodsServiceUnretryableFailure : une erreur inattendue et non renouvelable provenant du service WCF hébergé par MSODS s’est produite. [Ouvrez un ticket de support](../fundamentals/active-directory-troubleshooting-support-howto.md) pour plus d’informations sur l’erreur. |
| AADSTS90038 | NationalCloudTenantRedirection : le locataire spécifié « Y » appartient au Cloud National « X ». L’instance de cloud actuelle « Z » n’est pas fédérée avec X. Une erreur de redirection de cloud est retournée. |
| AADSTS90043 | NationalCloudAuthCodeRedirection : la fonctionnalité est désactivée. |
| AADSTS900432 | Le client confidentiel n’est pas pris en charge dans une requête intercloud.|
| AADSTS90051 | InvalidNationalCloudId : l’identificateur de cloud national contient un identificateur de cloud non valide. |
| AADSTS90055 | TenantThrottlingError : il y a un trop grand nombre de requêtes entrantes. Cette exception est levée pour les locataires bloqués. |
| AADSTS90056 | BadResourceRequest : pour utiliser le code pour un jeton d’accès, l’application doit envoyer une requête POST au point de terminaison `/token`. En outre, avant cela, vous devez fournir un code d’autorisation et l’envoyer dans la requête POST au point de terminaison `/token`. Consultez cet article pour une vue d’ensemble du flux du code d’autorisation OAuth 2.0 : [../azuread-dev/v1-protocols-oauth-code.md](../azuread-dev/v1-protocols-oauth-code.md). Dirigez l’utilisateur vers le point de terminaison `/authorize`, qui retournera un code d’autorisation. En publiant une requête pour le point de terminaison `/token`, l’utilisateur obtient le jeton d’accès. Ouvrez une session dans le portail Azure et vérifiez **Inscriptions d'applications > Points de terminaison** pour confirmer que les deux points de terminaison ont été configurés correctement. |
| AADSTS90072 | PassThroughUserMfaError : le compte externe avec lequel l’utilisateur se connecte n’existe pas sur le locataire auquel il s’était connecté ; par conséquent, l’utilisateur ne peut pas remplir les exigences d’authentification multifacteur pour le locataire. Cette erreur peut également se produire si les utilisateurs sont synchronisés, mais qu’il existe une incompatibilité dans l’attribut ImmutableID (sourceAnchor) entre Active Directory et Azure AD. Le compte doit d’abord être ajouté comme utilisateur externe dans le locataire. Déconnectez-vous puis reconnectez-vous avec un autre compte d’utilisateur Azure AD. |
| AADSTS90081 | OrgIdWsFederationMessageInvalid : une erreur s’est produite lorsque le service a tenté de traiter un message WS-Federation. Le message n'est pas valide. |
| AADSTS90082 | OrgIdWsFederationNotSupported : la stratégie d’authentification sélectionnée pour la demande n’est pas prise en charge actuellement. |
| AADSTS90084 | OrgIdWsFederationGuestNotAllowed : les comptes invités ne sont pas autorisés pour ce site. |
| AADSTS90085 | OrgIdWsFederationSltRedemptionFailed : le service ne peut pas émettre un jeton car l’objet de l’entreprise n’a pas encore été provisionné. |
| AADSTS90086 | OrgIdWsTrustDaTokenExpired : le jeton DA de l’utilisateur a expiré. |
| AADSTS90087 | OrgIdWsFederationMessageCreationFromUriFailed : une erreur s’est produite lors de la création du message WS-Federation à partir de l’URI. |
| AADSTS90090 | GraphRetryableError : le service est temporairement indisponible. |
| AADSTS90091 | GraphServiceUnreachable |
| AADSTS90092 | GraphNonRetryableError |
| AADSTS90093 | GraphUserUnauthorized : graphique retourné avec un code d’erreur interdit pour la demande. |
| AADSTS90094 | AdminConsentRequired : le consentement de l’administrateur est requis. |
| AADSTS900382 | Le client confidentiel n’est pas pris en charge dans une requête intercloud. |
| AADSTS90095  | AdminConsentRequiredRequestAccess : dans le cadre du flux de travail du consentement de l’administrateur, une interruption s’affiche lorsque l’utilisateur est informé qu’il doit demander son consentement à l’administrateur.  |
| AADSTS90099 | L’application « {appId} » ({appName}) n’a pas été autorisée dans le locataire « {tenant} ». Les applications doivent être autorisées à accéder au locataire du client pour pouvoir être utilisées par les administrateurs délégués partenaires. Fournissez un préconsentement ou exécutez l’API appropriée de l’Espace partenaires pour autoriser l’application. |
| AADSTS900971| Aucune adresse de réponse n’est fournie.|
| AADSTS90100 | InvalidRequestParameter : le paramètre est vide ou non valide. |
| AADSTS901002 | AADSTS901002 : Le paramètre de requête « resource » n’est pas pris en charge. |
| AADSTS90101 | InvalidEmailAddress : les données fournies ne correspondent pas à une adresse e-mail valide. L’adresse e-mail doit être au format `someone@example.com`. |
| AADSTS90102 | InvalidUriParameter : la valeur doit être un URI absolu valide. |
| AADSTS90107 | InvalidXml : la demande n’est pas valide. Assurez-vous que vos données ne comportent aucun caractère non valide.|
| AADSTS90114 | InvalidExpiryDate : l’horodatage d’expiration du jeton en bloc entraînera l’émission d’un jeton expiré. |
| AADSTS90117 | InvalidRequestInput |
| AADSTS90119 | InvalidUserCode : le code de l’utilisateur est null ou vide.|
| AADSTS90120 | InvalidDeviceFlowRequest : la demande a déjà été autorisée ou refusée. |
| AADSTS90121 | InvalidEmptyRequest : demande vide non valide.|
| AADSTS90123 | IdentityProviderAccessDenied : le jeton ne peut pas être émis car le fournisseur de l’identité ou de l’émission de la revendication a refusé la demande. |
| AADSTS90124 | V1ResourceV2GlobalEndpointNotSupported : la ressource n’est pas prise en charge sur les points de terminaison `/common` ou `/consumers`. Utilisez plutôt `/organizations` ou le point de terminaison propre au locataire. |
| AADSTS90125 | DebugModeEnrollTenantNotFound : utilisateur introuvable dans le système. Assurez-vous que vous avez correctement entré le nom de l’utilisateur. |
| AADSTS90126 | DebugModeEnrollTenantNotInferred : le type d’utilisateur n’est pas pris en charge sur ce point de terminaison. Le système ne peut pas déduire le locataire de l’utilisateur à partir de son nom. |
| AADSTS90130 | NonConvergedAppV2GlobalEndpointNotSupported : l’application n’est pas prise en charge sur les points de terminaison `/common` ou `/consumers`. Utilisez plutôt `/organizations` ou le point de terminaison propre au locataire. |
| AADSTS120000 | PasswordChangeIncorrectCurrentPassword |
| AADSTS120002 | PasswordChangeInvalidNewPasswordWeak |
| AADSTS120003 | PasswordChangeInvalidNewPasswordContainsMemberName |
| AADSTS120004 | PasswordChangeOnPremComplexity |
| AADSTS120005 | PasswordChangeOnPremSuccessCloudFail |
| AADSTS120008 | PasswordChangeAsyncJobStateTerminated : une erreur non renouvelable s’est produite.|
| AADSTS120011 | PasswordChangeAsyncUpnInferenceFailed |
| AADSTS120012 | PasswordChangeNeedsToHappenOnPrem |
| AADSTS120013 | PasswordChangeOnPremisesConnectivityFailure |
| AADSTS120014 | PasswordChangeOnPremUserAccountLockedOutOrDisabled |
| AADSTS120015 | PasswordChangeADAdminActionRequired |
| AADSTS120016 | PasswordChangeUserNotFoundBySspr |
| AADSTS120018 | PasswordChangePasswordDoesnotComplyFuzzyPolicy |
| AADSTS120020 | PasswordChangeFailure |
| AADSTS120021 | PartnerServiceSsprInternalServiceError |
| AADSTS130004 | NgcKeyNotFound : la clé d’identification NGC de l’utilisateur principal n’est pas configurée. |
| AADSTS130005 | NgcInvalidSignature : la vérification de la signature de la clé NGC a échoué.|
| AADSTS130006 | NgcTransportKeyNotFound : la clé de transport NGC n’est pas configurée sur l’appareil. |
| AADSTS130007 | NgcDeviceIsDisabled : l’appareil est désactivé. |
| AADSTS130008 | NgcDeviceIsNotFound : l’appareil référencé par la clé NGC est introuvable. |
| AADSTS135010 | KeyNotFound |
| AADSTS135011 |  L’appareil utilisé pendant l’authentification est désactivé.|
| AADSTS140000 | InvalidRequestNonce : aucune valeur à usage unique n’est fournie pour la demande. |
| AADSTS140001 | InvalidSessionKey : la clé de session n’est pas valide.|
| AADSTS165004 | Le contenu du message réel est spécifique au runtime. Consultez le message de l'exception renvoyé pour obtenir plus d'informations. |
| AADSTS165900 | InvalidApiRequest : demande non valide. |
| AADSTS220450 | UnsupportedAndroidWebViewVersion : la version de Chrome WebView n’est pas prise en charge. |
| AADSTS220501 | InvalidCrlDownload |
| AADSTS221000 | DeviceOnlyTokensNotSupportedByResource : la ressource n’est pas configurée pour accepter des jetons d’appareil uniquement. |
| AADSTS240001 | BulkAADJTokenUnauthorized : l’utilisateur n’est pas autorisé à inscrire des appareils dans Azure AD. |
| AADSTS240002 | RequiredClaimIsMissing : id_token ne peut pas être utilisé comme octroi `urn:ietf:params:oauth:grant-type:jwt-bearer`.|
| AADSTS530032 | BlockedByConditionalAccessOnSecurityPolicy : l’administrateur de locataire a configuré une stratégie de sécurité qui bloque cette requête. Vérifiez les stratégies de sécurité définies au niveau du locataire pour déterminer si votre requête répond aux exigences de la stratégie. |
| AADSTS700016 | UnauthorizedClient_DoesNotMatchRequest : l’application est introuvable dans le répertoire/locataire. Cela peut se produire si l’application n’a pas été installée par l’administrateur du locataire ni acceptée par un utilisateur dans le locataire. Vous avez peut-être configuré de manière incorrecte la valeur d’identificateur de l’application ou envoyé votre requête d’authentification à un locataire incorrect. |
| AADSTS700020 | InteractionRequired : l’octroi d’accès nécessite une interaction. |
| AADSTS700022 | InvalidMultipleResourcesScope : la valeur fournie pour l’étendue du paramètre d’entrée n’est pas valide car elle contient plusieurs ressources. |
| AADSTS700023 | InvalidResourcelessScope : la valeur fournie pour l’étendue du paramètre d’entrée n’est pas valide pour demander un jeton d’accès. |
| AADSTS7000215 | La clé secrète client fournie n’est pas valide. Erreur du développeur : l’application tente de se connecter sans les paramètres d’authentification nécessaires ou corrects.|
| AADSTS7000222 | InvalidClientSecretExpiredKeysProvided : les clés secrètes client fournies ont expiré. Visitez le portail Azure afin de créer de nouvelles clés pour votre application, ou envisagez d’utiliser des informations d’identification de certificat pour renforcer la sécurité : [https://aka.ms/certCreds](./active-directory-certificate-credentials.md) |
| AADSTS700005 | InvalidGrantRedeemAgainstWrongTenant : le code d’autorisation fourni est destiné à être utilisé auprès d’un autre locataire et a donc été rejeté. Le code d’autorisation OAuth2 doit être échangé auprès du même locataire pour lequel il a été acquis (/common ou /{tenant-ID}, le cas échéant) |
| AADSTS1000000 | UserNotBoundError : l’API Bind nécessite que l’utilisateur Azure AD s’authentifie également auprès d’un fournisseur d’identité externe, ce qui n’a pas encore été effectué. |
| AADSTS1000002 | BindCompleteInterruptError : la liaison s’est terminée correctement, mais l’utilisateur doit être informé. |
| AADSTS100007 | Les régions AAD prennent UNIQUEMENT en charge l’authentification pour les fichiers MSI ou pour les demandes de MSAL à l’aide de SN+I pour les applications 1P ou 3P dans les locataires de l’infrastructure Microsoft.|
| AADSTS1000031 | L’application {appDisplayName} n’est pas accessible pour l’instant. Contactez votre administrateur. |
| AADSTS7000112 | UnauthorizedClientApplicationDisabled : l’application est désactivée. |
| AADSTS7000114| L’application « appIdentifier » n’est pas autorisée à effectuer des appels au nom de l’application.|
| AADSTS7500529 | La valeur « SAMLId-GUID » n’est pas un ID SAML valide - Azure AD utilise cet attribut pour remplir l’attribut InResponseTo de la réponse retournée. L’ID ne doit pas commencer par un nombre ; vous pouvez donc suivre la stratégie courante qui consiste à ajouter une chaîne de type « id » devant la représentation sous forme de chaîne d’un GUID. Par exemple, id6c1c178c166d486687be4aaf5e482730 est un ID valide. |

## <a name="next-steps"></a>Étapes suivantes

* Vous avez une question ou vous ne trouvez pas ce que vous recherchez ? Créez un problème GitHub ou consultez [Options d’aide et de support pour les développeurs](./developer-support-help-options.md) pour en savoir plus sur les autres méthodes vous permettant d’obtenir de l’aide et un support.
