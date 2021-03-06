<properties 
	pageTitle="Débogage d'un service cloud ou d'une machine virtuelle Azure dans Visual Studio | Microsoft Azure"
	description="Débogage d’un service cloud ou d’une machine virtuelle dans Visual Studio"
	services="visual-studio-online"
	documentationCenter="na"
	authors="TomArcher"
	manager="douge"
	editor="" />
<tags 
	ms.service="visual-studio-online"
	ms.devlang="multiple"
	ms.topic="article"
	ms.tgt_pltfrm="multiple"
	ms.workload="na"
	ms.date="08/15/2016"
	ms.author="tarcher" />

# Débogage d'un service cloud ou d'une machine virtuelle Azure dans Visual Studio

Visual Studio vous offre différentes options de débogage des machines virtuelles et services cloud Azure.



## Déboguer votre service cloud sur votre ordinateur local

L’utilisation de l’émulateur de calcul Azure pour déboguer votre service cloud sur une machine locale constitue un gain de temps et d’argent. Déboguer un service localement avant de le déployer vous permet d’améliorer la fiabilité et les performances, tout en évitant les coûts de temps de calcul. Certaines erreurs peuvent toutefois se produire quand vous exécutez un service cloud directement dans Azure. Vous pouvez déboguer ces erreurs si vous activez le débogage distant quand vous publiez votre service, puis attachez le débogueur à une instance de rôle.

L’émulateur simule le service de calcul Azure et s’exécute dans votre environnement local pour vous permettre de tester et déboguer votre service cloud avant son déploiement. Il gère le cycle de vie de vos instances de rôle et donne accès aux ressources simulées, telles que le stockage local. Quand vous déboguez ou exécutez votre service à partir de Visual Studio, le programme démarre automatiquement l’émulateur comme une application d’arrière-plan, puis déploie votre service vers l’émulateur. Vous pouvez utiliser l’émulateur pour voir votre service en cours d’exécution dans l’environnement local. Vous pouvez exécuter la version complète ou express de l’émulateur. (À partir d’Azure 2.3, la version express de l’émulateur est celle par défaut.) Consultez [Utilisation de l’émulateur express pour exécuter et déboguer un service cloud localement](https://msdn.microsoft.com/library/dn339018.aspx).

### Pour déboguer votre service cloud sur votre ordinateur local

1. Dans la barre de menus, sélectionnez **Déboguer**, **Démarrer le débogage** pour exécuter votre projet de service cloud Azure. Vous pouvez aussi appuyer sur F5. Un message s’affiche, indiquant que l’émulateur de calcul est en cours de démarrage. Une icône s’affiche dans la zone de notification pour confirmer le démarrage de l’émulateur.

    ![Émulateur Azure dans la zone de notification](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC783828.png)

1. Affichez l’interface utilisateur de l’émulateur de calcul en ouvrant le menu contextuel de l’icône Azure dans la zone de notification, puis en sélectionnant **Afficher l’interface utilisateur de l’émulateur de calcul**.

    Le volet gauche de l'interface utilisateur montre les services actuellement déployés sur l'émulateur de calcul et les instances de rôle en cours d'exécution sur chaque service. Vous pouvez sélectionner le service ou des rôles pour afficher les informations liées au cycle de vie, aux journaux et aux diagnostics dans le volet de droite. Si vous placez le focus dans la marge supérieure d’une fenêtre incluse, celle-ci se développe pour remplir le volet droit.

1. Parcourez l’application en sélectionnant les commandes du menu **Déboguer** et en définissant des points d’arrêt dans votre code. À mesure que vous parcourez l'application dans le débogueur, les volets sont mis à jour avec l'état actuel de l'application. Quand vous arrêtez le débogage, le déploiement de l’application est annulé. Si votre application inclut un rôle web et que vous avez défini la propriété Action de démarrage pour démarrer le navigateur web, Visual Studio démarre votre application web dans le navigateur. Si vous modifiez le nombre d’instances d’un rôle dans la configuration du service, vous devez arrêter votre service cloud, puis relancer le débogage pour déboguer ces nouvelles instances du rôle.

    **Remarque** : quand vous arrêtez l’exécution ou le débogage de votre service, l’émulateur de calcul et l’émulateur de stockage locaux ne sont pas interrompus. Vous devez les arrêter explicitement à partir de la zone de notification.


## Déboguer un service cloud dans Azure

Pour déboguer un service cloud à partir d’une machine distante, vous devez activer cette fonctionnalité de façon explicite au moment du déploiement de ce service. Tous les services nécessaires (msvsmon.exe, par exemple) sont alors installés sur les machines virtuelles qui exécutent vos instances de rôle. Si vous n’avez pas activé le débogage distant quand vous avez publié le service, vous devez republier ce service avec le débogage distant activé.

L’activation du débogage distant pour un service cloud n’entraîne pas de baisse des performances, ni de coûts supplémentaires. Vous ne devez pas utiliser le débogage distant pour un service de production, car cela peut avoir un impact pour les clients qui utilisent ce service.

>[AZURE.NOTE] Quand vous publiez un service cloud à partir de Visual Studio, vous pouvez activer **IntelliTrace** pour tous les rôles de ce service qui ciblent .NET Framework 4 ou .NET Framework 4.5. Avec **IntelliTrace**, vous pouvez examiner des événements qui se sont déjà produits dans une instance de rôle et reproduire le contexte de ce moment-là. Consultez [Débogage d’un service cloud publié avec IntelliTrace et Visual Studio](http://go.microsoft.com/fwlink/?LinkID=623016) et [Utilisation d’IntelliTrace](https://msdn.microsoft.com/library/dd264915.aspx).

### Pour activer le débogage distant pour un service cloud

1. Ouvrez le menu contextuel du projet Azure, puis sélectionnez **Publier**.

1. Sélectionnez l’environnement **Intermédiaire** et la configuration **Débogage**.

    Il s’agit simplement d’une indication. Vous pouvez choisir d’exécuter vos environnements de test dans un environnement de production. L’activation du débogage distant sur l’environnement de production risque toutefois d’avoir un impact pour les utilisateurs. Vous pouvez choisir la configuration Release, mais la configuration Debug présente l’avantage de simplifier le débogage.

    ![Choisir la configuration Debug](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746717.gif)

1. Suivez les étapes habituelles, en veillant à cocher la case **Activer le débogueur distant pour tous les rôles** dans l’onglet **Paramètres avancés**.

    ![Configuration Debug](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746718.gif)

### Pour attacher le débogueur à un service cloud dans Azure

1. Dans l’Explorateur de serveurs, développez le nœud de votre service cloud.

1. Ouvrez le menu contextuel du rôle ou de l’instance de rôle auquel vous souhaitez joindre le débogueur, puis sélectionnez **Attacher le débogueur**.

    Si vous déboguez un rôle, le débogueur Visual Studio s'attache à chaque instance de ce rôle. Le débogueur s’arrêtera sur un point d’arrêt pour la première instance de rôle qui exécute cette ligne de code et remplit les conditions de ce point d’arrêt. Si vous déboguez une instance, le débogueur est attaché uniquement à cette instance, et s’arrête sur un point d’arrêt quand cette instance spécifique exécute cette ligne de code et remplit les conditions du point d’arrêt.

    ![Attacher le débogueur](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746719.gif)

1. Après avoir attaché le débogueur à une instance, vous pouvez procéder au débogage comme vous le faites habituellement. Le débogueur est automatiquement attaché au processus hôte approprié pour votre rôle. Selon le rôle, le débogueur est attaché à w3wp.exe, WaWorkerHost.exe ou WaIISHost.exe. Pour identifier le processus auquel le débogueur est attaché, développez le nœud de l’instance dans l’Explorateur de serveurs. Pour plus d’informations sur les processus Azure, consultez [Architecture de rôle Azure](http://blogs.msdn.com/b/kwill/archive/2011/05/05/windows-azure-role-architecture.aspx).

    ![Boîte de dialogue Sélectionner le type de code](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)

1. Pour identifier les processus auxquels le débogueur est attaché, sélectionnez Déboguer, Windows, Processus dans la barre de menus pour ouvrir la boîte de dialogue Processus (ou appuyez sur Ctrl+Alt+Z). Pour détacher un processus spécifique, ouvrez son menu contextuel, puis sélectionnez **Détacher le processus**. Vous pouvez aussi afficher le nœud d’instance dans l’Explorateur de serveurs, rechercher le processus et ouvrir son menu contextuel, puis sélectionnez **Détacher le processus**.

    ![Processus de débogage](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC690787.gif)

>[AZURE.WARNING] Évitez les arrêts longs aux points d'arrêt avec le débogage à distance. Azure considère qu’un processus arrêté depuis plusieurs minutes ne répond pas, et cesse d’envoyer du trafic vers cette instance. Si l’arrêt est trop long, msvsmon.exe est détaché du processus.

Pour détacher le débogueur de tous les processus dans votre instance ou rôle, ouvrez le menu contextuel du rôle ou de l’instance que vous déboguez, puis sélectionnez **Détacher le débogueur**.

## Limitations du débogage distant dans Azure

À partir de la version 2.3 du Kit de développement logiciel (SDK), le débogage à distance présente les limitations ci-dessous.

- Quand le débogage distant est activé, vous ne pouvez pas publier un service cloud dans lequel un rôle comporte plus de 25 instances.

- Le débogueur utilise les ports 30400 à 30424, 31400 à 31424 et 32400 à 32424. Si vous essayez d’utiliser ces ports, vous ne pourrez pas publier votre service, et l’un des messages d’erreur suivants s’affichera dans le journal des activités Azure :

    - Erreur lors de la validation du fichier .cscfg par rapport au fichier .csdef. La plage de ports réservée ('range') pour le point de terminaison Microsoft.WindowsAzure.Plugins.RemoteDebugger.Connector du rôle ('role') chevauche une plage ou un port déjà défini.
    - L’allocation a échoué. Réessayez ultérieurement, essayez de réduire la taille de la machine virtuelle ou le nombre d’instances de rôle, ou essayez de déployer dans une autre région.


## Déboguer des machines virtuelles Azure

Vous pouvez déboguer des programmes exécutés sur des machines virtuelles Azure à l’aide de l’Explorateur de serveurs dans Visual Studio. Quand vous activez le débogage distant sur une machine virtuelle Azure, Azure installe l’extension de débogage distant sur cette machine virtuelle. Vous pouvez ensuite l’attacher aux processus sur la machine virtuelle et procéder au débogage normalement.

>[AZURE.NOTE] Les machines virtuelles créées via la pile Azure Resource Manager peuvent être déboguées à distance à l’aide de Cloud Explorer dans Visual Studio 2015. Pour plus d’informations, consultez [Gestion des ressources Azure avec Cloud Explorer](http://go.microsoft.com/fwlink/?LinkId=623031).

### Pour déboguer une machine virtuelle Azure

1. Dans l’Explorateur de serveurs, développez le nœud Machines virtuelles, puis sélectionnez le nœud de la machine virtuelle à déboguer.

1. Ouvrez le menu contextuel, puis sélectionnez **Activer le débogage**. Quand vous êtes invité à confirmer l’activation du débogage sur la machine virtuelle, sélectionnez **Oui**.

    Azure installe l’extension de débogage distant sur la machine virtuelle pour activer le débogage.

    ![Commande d’activation du débogage de machine virtuelle](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746720.png)

    ![Journal des activités Azure](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746721.png)

1. Une fois l’extension de débogage à distance installée, ouvrez le menu contextuel de la machine virtuelle et sélectionnez **Attacher le débogueur...**

    Azure récupère la liste des processus sur la machine virtuelle et l’affiche dans la boîte de dialogue Attacher au processus.

    ![Commande Attacher le débogueur](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746722.png)

1. Dans la boîte de dialogue **Attacher au processus**, sélectionnez **Sélectionner** pour limiter la liste des résultats et afficher uniquement les types de code que vous voulez déboguer. Vous pouvez déboguer du code managé 32 ou 64 bits, du code natif ou les deux.

    ![Boîte de dialogue Sélectionner le type de code](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)

1. Sélectionnez les processus que vous voulez déboguer sur la machine virtuelle, puis **Attacher**. Par exemple, vous pouvez choisir le processus w3wp.exe pour déboguer une application web sur la machine virtuelle. Pour plus d’informations, consultez [Déboguer un ou plusieurs processus dans Visual Studio](https://msdn.microsoft.com/library/jj919165.aspx) et [Architecture de rôle Azure](http://blogs.msdn.com/b/kwill/archive/2011/05/05/windows-azure-role-architecture.aspx).

## Créer un projet web et une machine virtuelle pour le débogage

Avant de publier votre projet Azure, il peut être utile de le tester dans un environnement contrôlé qui prend en charge les scénarios de test et de débogage, et où vous pouvez installer les programmes de test et de surveillance. Pour ce faire, vous pouvez déboguer votre application à distance sur une machine virtuelle.

Les projets Visual Studio ASP.NET permettent de créer une machine virtuelle que vous pouvez utiliser pour tester les applications. La machine virtuelle inclut les points de terminaison généralement requis tels que PowerShell, le Bureau à distance et Web Deploy.

### Pour créer un projet web et une machine virtuelle pour le débogage

1. Dans Visual Studio, créez une application web ASP.NET.

1. Dans la boîte de dialogue Nouveau projet ASP.NET, dans la section Azure, sélectionnez **Machine virtuelle** dans la liste déroulante. Laissez la case **Créer des ressources distantes** cochée. Sélectionnez **OK** pour poursuivre.

    La boîte de dialogue **Créer une machine virtuelle dans Azure** s’affiche.


    ![Boîte de dialogue de création de projet web ASP.NET](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746723.png)

    **Remarque** : vous serez invité à vous connecter à votre compte Azure si vous ne l’êtes pas déjà.

1. Sélectionnez les différents paramètres de la machine virtuelle, puis sélectionnez **OK**. Pour plus d’informations, consultez [Machines virtuelles](http://go.microsoft.com/fwlink/?LinkId=623033).

    Le nom que vous entrez dans le champ Nom DNS sera le nom de votre machine virtuelle.

    ![Boîte de dialogue Créer une machine virtuelle sur Microsoft Azure](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746724.png)

    Azure crée la machine virtuelle, puis déploie et configure les points de terminaison, tels que le Bureau à distance et Web Deploy.



1. Une fois la machine virtuelle entièrement configurée, sélectionnez son nœud dans l’Explorateur de serveurs.

1. Ouvrez le menu contextuel, puis sélectionnez **Activer le débogage**. Quand vous êtes invité à confirmer l’activation du débogage sur la machine virtuelle, sélectionnez **Oui**.

    Azure installe l’extension de débogage distant sur la machine virtuelle pour activer le débogage.

    ![Commande d’activation du débogage de machine virtuelle](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746720.png)

    ![Journal des activités Azure](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746721.png)

1. Publiez votre projet comme décrit dans la rubrique [Déploiement d’un projet web à l’aide de la publication en un clic dans Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx). Comme vous voulez effectuer le débogage sur la machine virtuelle, sur la page **Paramètres** de l’Assistant **Publier le site web**, sélectionnez la configuration **Débogage**. Ceci permet de s’assurer que les symboles de code sont disponibles pendant le débogage.

    ![Paramètres de publication](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718349.png)

1. Dans **Options de publication des fichiers**, sélectionnez **Supprimer les fichiers supplémentaires à la destination** si le projet a déjà été déployé précédemment.

1. Une fois le projet publié, dans le menu contextuel de la machine virtuelle dans l’Explorateur de serveurs, sélectionnez **Attacher le débogueur...**

    Azure récupère la liste des processus sur la machine virtuelle et l’affiche dans la boîte de dialogue Attacher au processus.

    ![Commande Attacher le débogueur](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746722.png)

1. Dans la boîte de dialogue **Attacher au processus**, sélectionnez **Sélectionner** pour limiter la liste des résultats et afficher uniquement les types de code que vous voulez déboguer. Vous pouvez déboguer du code managé 32 ou 64 bits, du code natif ou les deux.

    ![Boîte de dialogue Sélectionner le type de code](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)

1. Sélectionnez les processus que vous voulez déboguer sur la machine virtuelle, puis **Attacher**. Par exemple, vous pouvez choisir le processus w3wp.exe pour déboguer une application web sur la machine virtuelle. Pour plus d’informations, consultez [Déboguer un ou plusieurs processus dans Visual Studio](https://msdn.microsoft.com/library/jj919165.aspx).

## Étapes suivantes

- Utilisez **IntelliTrace** pour collecter un journal des appels et des événements d’un serveur de publication. Consultez [Débogage d’un service cloud publié avec IntelliTrace et Visual Studio](http://go.microsoft.com/fwlink/?LinkID=623016).
- Utilisez les **Diagnostics Azure** pour collecter des informations détaillées du code actuellement exécuté dans des rôles qui sont eux-mêmes exécutés dans l’environnement de développement ou dans Azure. Consultez [Collecter des données de journalisation avec les diagnostics Azure](http://go.microsoft.com/fwlink/p/?LinkId=400450).

<!---HONumber=AcomDC_0817_2016-->