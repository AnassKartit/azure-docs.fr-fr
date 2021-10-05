---
title: Exemples de types de classes dans Azure Lab Services | Microsoft Docs
description: Cite certains des types de classes pour lesquels il est possible de configurer des laboratoires à l’aide d’Azure Lab Services.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: 086f5eebb6ef96b1846c2f2777b045821b0418c8
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2021
ms.locfileid: "124736634"
---
# <a name="class-types-overview---azure-lab-services"></a>Vue d’ensemble des types de classes - Azure Lab Services

Azure Lab Services vous permet de configurer rapidement un environnement lab de salle de classe dans le cloud. Les articles de cette section fournissent des conseils sur la configuration de plusieurs types de labos avec Azure Lab Services.

## <a name="adobe-creative-cloud"></a>Adobe Creative Cloud

L'ensemble d'applications [Adobe Creative Cloud](https://www.adobe.com/creativecloud.html) est couramment utilisé dans les cours dédiés aux arts numériques et au multimédia.  

Pour plus d'informations sur la configuration de ce type de labo, consultez [Configurer un labo pour Adobe Creative Cloud](class-type-adobe-creative-cloud.md).

## <a name="arcgis"></a>ArcGIS

[ArcGIS](https://www.esri.com/en-us/arcgis/products/arcgis-solutions/overview) est un type de système d’information géographique (SIG).  Vous pouvez configurer un labo qui utilise diverses applications du Bureau ArcGIS, telles que [ArcMap](https://desktop.arcgis.com/en/arcmap/latest/map/main/what-is-arcmap-.htm) pour créer, modifier et analyser des cartes 2D.

Pour plus d’informations sur la configuration de ce type de labo, consultez [Configurer un labo pour le Bureau ArcMap\ArcGIS](class-type-arcgis.md).

## <a name="autodesk"></a>Autodesk

[Autodesk](https://www.autodesk.com/) propose des solutions logicielles dans les domaines de l’architecture, de l’ingénierie, de la construction, de la conception, de la fabrication, etc.  Ces solutions sont couramment utilisées dans les cours d'ingénierie et dans le cadre du programme [Project Lead the Way](class-type-pltw.md).

Pour plus d'informations sur la configuration de ce type de labo, consultez [Autodesk](class-type-autodesk.md).

## <a name="big-data-analytics"></a>Analytique du Big Data

Vous pouvez configurer un labo de GPU pour enseigner une classe Analytique du Big Data. Grâce à ce type de classe, les étudiants apprennent à gérer de gros volumes de données et à appliquer des algorithmes d’apprentissage automatique et statistique pour obtenir des informations sur les données. L’un des principaux objectifs pour les étudiants est d’apprendre à utiliser les outils d’Analytique données, tels que le package logiciel open source d’Apache Hadoop qui fournit des outils pour le stockage, la gestion et le traitement du Big Data. 

Pour plus d’informations sur la façon de configurer ce type de labo, consultez [Configurer un labo pour l’analytique du Big Data à l’aide du déploiement Docker de Hortonworks Data Platform](class-type-big-data-analytics.md).

## <a name="database-management"></a>Gestion de bases de données

Les concepts des bases de données constituent l’un des cours d’introduction abordés dans la plupart des départements informatiques à l’université. Vous pouvez configurer un labo pour un cours élémentaire de gestion de bases de données dans Azure Lab Services. Par exemple, vous pouvez configurer un modèle de machine virtuelle dans un labo avec un serveur de base de données [MySQL](https://www.mysql.com/) ou un serveur [SQL Server 2019](https://www.microsoft.com/sql-server/sql-server-2019).

Pour des informations détaillées sur la configuration de ce type de labo, consultez [Configurer un labo pour enseigner la gestion des bases de données pour les bases de données relationnelles](class-type-database-management.md).

## <a name="deep-learning-in-natural-language-processing"></a>Deep Learning pour le traitement en langage naturel

Vous pouvez configurer un laboratoire axé sur le Deep Learning dans le cadre du traitement en langage naturel (NLP) à l’aide d’Azure Lab Services. Le traitement en langage naturel (NLP) est une forme d’intelligence artificielle (IA) qui fournit aux ordinateurs des fonctionnalités de traduction, de reconnaissance vocale et d’autres fonctionnalités de compréhension de la langue. Les étudiants qui suivent un cours de traitement en langage naturel disposent d’une machine virtuelle Linux pour apprendre à appliquer des algorithmes de réseau neuronal afin de développer des modèles de deep learning utilisés pour l’analyse de la langue humaine écrite.

Pour plus d’informations sur la configuration de ce type de labo, consultez [Configurer un labo axé sur le Deep Learning pour le traitement en langage naturel à l’aide d’Azure Lab Services](class-type-deep-learning-natural-language-processing.md).

## <a name="ethical-hacking-with-hyper-v"></a>Piratage éthique avec Hyper-V

Vous pouvez configurer un labo de classe qui se concentre sur l’analyse forensique du piratage éthique. Des tests d’intrusion, une pratique utilisée par la communauté de piratage éthique, sont effectués quand quelqu’un tente d’accéder au système ou au réseau pour détecter les vulnérabilités qu’un attaquant malveillant pourrait exploiter.

Dans une classe sur le piratage éthique, les étudiants apprennent les techniques modernes de défense face aux vulnérabilités. Chaque étudiant a accès à une machine virtuelle hôte Windows Server qui comporte deux machines virtuelles imbriquées : une avec une image [Metasploitable3](https://github.com/rapid7/metasploitable3) et une autre avec une image [Kali Linux](https://www.kali.org/). La machine virtuelle Metasploitable est utilisée à des fins d’exploitation.  La machine virtuelle Kali Linux permet d’accéder aux outils nécessaires pour exécuter des tâches d’investigation.

Pour plus d’informations sur la configuration de ce type de labo, consultez [Configurer un labo pour enseigner le piratage éthique](class-type-ethical-hacking.md).

## <a name="ethical-hacking-with-virtualbox"></a>Piratage éthique avec VirtualBox

Vous pouvez configurer un labo de classe qui se concentre sur l’analyse forensique du piratage éthique. Des tests d’intrusion, une pratique utilisée par la communauté de piratage éthique, sont effectués quand quelqu’un tente d’accéder au système ou au réseau pour détecter les vulnérabilités qu’un attaquant malveillant pourrait exploiter.

Dans une classe sur le piratage éthique, les étudiants apprennent les techniques modernes de défense face aux vulnérabilités. Chaque étudiant a accès à une machine virtuelle hôte Windows Server qui a deux machines virtuelles imbriquées : une machine virtuelle avec une image [SEED Labs](https://seedsecuritylabs.org/) et une autre machine avec une image [Kali Linux](https://www.kali.org/). La machine virtuelle SEED est utilisée à des fins d’exploitation.  La machine virtuelle Kali Linux permet d’accéder aux outils nécessaires pour exécuter des tâches d’investigation.

Pour plus d’informations sur la configuration de ce type de labo, consultez [Configurer un labo pour enseigner le piratage éthique](class-type-ethical-hacking-virtualbox.md).

## <a name="matlab"></a>MATLAB

[MATLAB](https://www.mathworks.com/products/matlab.html), qui signifie Matrix Laboratory, est la plate-forme de programmation de [MathWorks](https://www.mathworks.com/).  Elle combine la puissance de calcul et la visualisation, ce qui en fait un outil populaire dans les domaines des mathématiques, de l’ingénierie, de la physique et de la chimie.

Pour plus d’informations sur la configuration de ce type de labo, consultez [Configurer un labo pour enseigner MATLAB](class-type-matlab.md).

## <a name="networking-with-gns3"></a>Réseau avec GNS3

Vous pouvez configurer un labo pour une classe qui se concentre sur la possibilité pour les étudiants d’émuler, de configurer, de tester et de dépanner des réseaux virtuels et réels à l’aide du logiciel [GNS3](https://www.gns3.com/). 

Pour plus d’informations sur la configuration de ce type de labo, consultez [Configurer un labo pour enseigner une classe réseau](class-type-networking-gns3.md).

## <a name="project-lead-the-way-pltw"></a>Project Lead the Way (PLTW)

[Project Lead the Way (PLTW)](https://www.pltw.org/) est un organisme à but non lucratif qui propose des programmes éducatifs à l’intention des élèves du primaire à travers les États-Unis dans les domaines de l’informatique, de l’ingénierie et des sciences biomédicales.  Dans chaque cours PLTW, les étudiants utilisent diverses applications logicielles dans le cadre de leurs travaux pratiques.

Pour plus d’informations sur la façon de configurer ces types de laboratoires, consultez [Configurer des labos pour des cours Project Lead the Way](class-type-pltw.md).

## <a name="python-and-jupyter-notebooks"></a>Python et Jupyter Notebooks

Vous pouvez configurer une machine modèle dans Azure Lab Services avec les outils nécessaires pour enseigner aux élèves comment utiliser [Jupyter Notebooks](http://jupyter-notebook.readthedocs.io). Jupyter Notebooks est un projet open source qui vous permet de combiner facilement du texte enrichi et du code source [Python](https://www.python.org/) exécutable sur un seul canevas appelé notebook. L’exécution d’un notebook produit un enregistrement linéaire des entrées et des sorties.  Ces sorties peuvent inclure du texte, des tables d’informations, des nuages de points, etc.

Pour des informations détaillées sur la configuration de ce type de labo, consultez [Configurer un labo pour enseigner la science des données avec Python et Jupyter Notebooks](class-type-jupyter-notebook.md).

## <a name="react"></a>React

[React](https://reactjs.org/) est une bibliothèque JavaScript couramment utilisée pour créer des interfaces utilisateur (IU). React est un moyen déclaratif de créer des composants réutilisables pour votre site web.  De nombreuses bibliothèques sont disponibles pour le développement front-end basé sur JavaScript.  Nous utiliserons quelques-unes de ces bibliothèques lors de la création de notre labo.  [Redux](https://redux.js.org/) est une bibliothèque qui fournit un conteneur d'état prévisible pour les applications JavaScript. Elle est souvent utilisée en complément de React. [JSX](https://reactjs.org/docs/introducing-jsx.html) est une extension syntaxique de la bibliothèque JavaScript souvent utilisée avec React pour décrire ce à quoi l'interface utilisateur doit ressembler.  [NodeJS](https://nodejs.org/) est un moyen pratique d'exécuter un serveur web pour votre application React.

Pour plus d’informations sur la configuration de ce type de labo sur Linux à l’aide de [Visual Studio Code](https://code.visualstudio.com/) pour votre environnement de développement, consultez [Configurer un labo pour React sur Windows](class-type-react-linux.md).  Pour plus d’informations sur la configuration de ce type de labo sur Windows à l’aide de [Visual Studio 2019](https://visualstudio.microsoft.com/vs/) pour votre environnement de développement, consultez [Configurer un labo pour React sur Windows](class-type-react-windows.md).

## <a name="rstudio"></a>RStudio

[R](https://www.r-project.org/about.html) est un langage open source utilisé pour le calcul statistique et les graphiques.  Il est utilisé à des fins d’analyse statistique de la génétique, de traitement du langage naturel, d’analyse des données financières, etc.  Le langage R offre une expérience de [ligne de commande interactive](https://cran.r-project.org/doc/manuals/r-release/R-intro.html#Invoking-R-from-the-command-line).  [RStudio](https://www.rstudio.com/products/rstudio/) est un environnement de développement intégré (IDE) interactif disponible pour le langage R.  La version gratuite fournit des outils d’édition de code, une expérience de débogage intégrée et des outils de développement de package.  Ce type de classe se concentre uniquement sur RStudio et R en tant que bloc de construction pour une classe nécessitant l’utilisation du calcul statistique.

Pour plus d’informations sur la configuration de ce type de labo, consultez [Configurer un labo pour enseigner R sur Linux](class-type-rstudio-linux.md) ou [Configurer un labo pour enseigner R sur Windows](class-type-rstudio-windows.md).

## <a name="shell-scripting-on-linux"></a>Scripts shell sur Linux

Vous pouvez configurer un laboratoire pour enseigner la création de scripts shell sur Linux. Dans le cadre de l’administration système, l’écriture de scripts permet aux administrateurs d’éviter les tâches répétitives. Dans cet exemple de scénario, les scripts bash traditionnels et les scripts améliorés sont abordés. Les scripts améliorés sont des scripts qui associent des commandes bash et Ruby. Cette approche permet à Ruby de passer des données et fournit des commandes bash pour interagir avec le shell.

Les étudiants qui suivent ces cours d’écriture de scripts disposent d’une machine virtuelle Linux pour apprendre les bases de Linux et également se familiariser avec les scripts de shell bash. La machine virtuelle Linux est fournie avec un accès au Bureau à distance activé. En outre, les éditeurs de texte [Gedit](https://help.gnome.org/users/gedit/stable/) et [Visual Studio Code](https://code.visualstudio.com/) y sont installés.

Pour plus d’informations sur la configuration de ce type de labo, consultez [Scripts shell sur Linux](class-type-shell-scripting-linux.md).

## <a name="solidworks-computer-aided-design-cad"></a>Conception assistée par ordinateur (CAO) SolidWorks

Vous pouvez configurer un laboratoire GPU qui donne aux étudiants d’ingénierie l’accès à [SolidWorks](https://www.solidworks.com/).  SolidWorks fournit un environnement de CAO 3D pour la modélisation des objets solides.  Avec SolidWorks, les ingénieurs peuvent facilement créer, visualiser, simuler et documenter leurs conceptions.

Pour plus d’informations sur la configuration de ce type de labo, consultez [Configurer un labo pour les cours d’ingénierie avec SolidWorks](class-type-solidworks.md).

## <a name="sql-database-and-management"></a>SQL Database et gestion

SQL est le langage standard pour la gestion des bases de données relationnelles, notamment l’ajout, la récupération et la gestion du contenu dans une base de données.  Vous pouvez configurer un labo pour enseigner les concepts de base de données à l’aide du serveur de base de données [MySQL](https://www.mysql.com/) et du serveur [SQL Server 2019](https://www.microsoft.com/sql-server/sql-server-2019).

Pour des informations détaillées sur la configuration de ce type de labo, consultez [Configurer un labo pour enseigner la gestion des bases de données pour les bases de données relationnelles](class-type-database-management.md).

## <a name="next-steps"></a>Étapes suivantes

Voir les articles suivants :

- [Configurer un laboratoire axé sur le Deep Learning dans le cadre du traitement en langage naturel (NLP) à l’aide d’Azure Lab Services](class-type-deep-learning-natural-language-processing.md)
- [Configurer un labo pour enseigner la mise en réseau](class-type-networking-gns3.md)
- [Configurer un labo pour enseigner le piratage éthique avec Hyper-V](class-type-ethical-hacking.md)
- [Configurer un labo pour enseigner le piratage éthique avec VirtualBox](class-type-ethical-hacking-virtualbox.md)
