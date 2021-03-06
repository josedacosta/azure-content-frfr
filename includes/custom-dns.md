﻿# Configuration d'un nom de domaine personnalisé pour un service cloud Azure

> [AZURE.NOTE]
> Avancez plus rapidement, utilisez la NOUVELLE [procédure d'Azure pas à pas](http://support.microsoft.com/kb/2990804) !  L'association d'un nom de domaine personnalisé ET la sécurisation des communications (SSL) avec Azure Cloud Services ou Sites Web Azure deviennent un jeu d'enfant.

Lorsque vous créez une application dans Azure, Azure fournit un sous-domaine du domaine cloudapp.net pour que vos utilisateurs puissent accéder à votre application via une URL telle que http://&lt;*monapplication*>. cloudapp.net. Toutefois, vous pouvez également exposer votre application sur votre propre nom de domaine, par exemple, contoso.com.

> [AZURE.NOTE] 
> Les procédures décrites dans cette tâche s'appliquent aux services cloud Azure. Pour les comptes de stockage, consultez la rubrique [Configuration d'un nom de domaine personnalisé pour un compte de stockage Azure](../articles/storage-custom-domain-name.md). Pour les sites web, consultez la rubrique [Configuration d'un nom de domaine personnalisé pour un site web Azure](../articles/web-sites-custom-domain-name.md).


## Présentation des enregistrements CNAME et A

Bien que fonctionnant différemment, les enregistrements CNAME (ou enregistrements d'alias) et A permettent d'associer un nom de domaine avec un serveur spécifique (ou un service dans ce cas). Avant de faire votre choix, certains aspects spécifiques sont également à prendre en considération lors de l'utilisation d'enregistrements A avec les services cloud Azure.

### Enregistrement CNAME ou d'alias

Un enregistrement CNAME mappe un domaine *specific*, tel que **contoso.com** ou **www.contoso.com**, à un nom de domaine canonique. Dans ce cas, le nom de domaine canonique est le nom de domaine  **&lt;myapp>.cloudapp.net** de votre application hébergée Azure. Une fois créé, l'enregistrement CNAME émet un alias pour **&lt;myapp>.cloudapp.net**. L'entrée CNAME devient automatiquement l'adresse IP de votre service **&lt;myapp>.cloudapp.net**. Ainsi, même si l'adresse IP du service cloud change, vous n'avez aucune action à effectuer.

> [AZURE.NOTE] 
> Certains bureaux d'enregistrement de domaines autorisent le mappage de sous-domaines uniquement lorsqu'un enregistrement CNAME est utilisé (par exemple, www.contoso.com) et non un nom racine (tel que contoso.com). Pour plus d'informations sur les enregistrements CNAME, consultez la documentation fournie par votre bureau d'enregistrement, <a href="http://en.wikipedia.org/wiki/CNAME_record">la page Wikipédia sur l'enregistrement CNAME</a> ou le document <a href="http://tools.ietf.org/html/rfc1035">Noms de domaine IETF - Implémentation et spécification</a>.

### Enregistrement A

Un enregistrement A mappe un domaine, tel que **contoso.com** ou **www.contoso.com** *or a wildcard domain* comme **.contoso.com**, à une adresse IP. Dans le cas d'un service cloud Azure, il s'agit de l'adresse IP virtuelle du service. Par conséquent, l'avantage principal d'un enregistrement A sur un enregistrement CNAME est que vous pouvez avoir une entrée utilisant un caractère générique, par exemple, ***.contoso.com**, qui permet de traiter les requêtes pour plusieurs sous-domaines tels que **mail.contoso.com**, **login.contoso.com** ou **www.contso.com**.

> [AZURE.NOTE]
> L'enregistrement A étant associé à une adresse IP statique, les changements d'adresse IP de votre service cloud ne sont donc pas pris en compte automatiquement. L'adresse IP utilisée par votre service cloud est allouée la première fois que vous effectuez un déploiement vers un emplacement vide (de production ou intermédiaire). Si vous supprimez le déploiement de l'emplacement, l'adresse IP est publiée par Azure et tout déploiement futur dans l'emplacement peut recevoir une nouvelle adresse IP.
> 
> De façon assez pratique, l'adresse IP d'un emplacement de déploiement donné (de production ou intermédiaire) est conservée lors du basculement entre les déploiements intermédiaires et de production ou lors de l'exécution de la mise à niveau sur place d'un déploiement existant. Pour plus d'informations sur ces actions, consultez la rubrique [Gestion des services cloud](../articles/cloud-services-how-to-manage.md).


## Ajout d'un enregistrement CNAME pour votre domaine personnalisé

Pour créer un enregistrement CNAME, vous devez ajouter une nouvelle entrée dans la table DNS de votre domaine personnalisé en utilisant les outils fournis par votre bureau d'enregistrement. Chaque bureau d'enregistrement possède sa propre méthode de spécification des enregistrements CNAME, même si le fonctionnement général reste souvent similaire.

1. Employez une des méthodes suivantes pour connaître le nom de domaine **.cloudapp.net** attribué à votre service cloud.

  * Connectez-vous au [portail de gestion Azure], sélectionnez votre service cloud, sélectionnez **Tableau de bord**, puis recherchez l'entrée **URL du site** dans la section **Aperçu rapide**.

  		  ![section Aperçu rapide affichant l'URL du site][csurl]

  * Installez et configurez [Azure Powershell](../articles/install-configure-powershell.md), puis utilisez la commande suivante :

    Get-AzureDeployment -ServiceName votre_nom_service | Select Url

  Enregistrez le nom de domaine utilisé dans l'URL renvoyée par l'une des méthodes, car vous en aurez besoin lors de la création d'un enregistrement CNAME.

1.  Connectez-vous au site web du bureau d'enregistrement de votre DNS et accédez à la page de gestion DNS. Recherchez des liens ou des zones indiquant **Nom de domaine**, **DNS** ou **Name Server Management**.

2.  Maintenant, cherchez où vous pouvez sélectionner ou saisir vos enregistrements CNAME. Il se peut que vous deviez sélectionner le type d'enregistrement dans une liste déroulante ou accéder à une page de paramètres avancés. La section recherchée doit normalement comporter les mots **CNAME**, **Alias** ou **sous-domaines**.

3.  Vous devez également fournir l'alias de domaine ou de sous-domaine pour l'enregistrement CNAME, tel que **www** si vous souhaitez créer un alias pour **www.domainepersonnalisé.com**. Si vous souhaitez créer un alias pour le domaine racine, l'entrée correspondante devrait être répertoriée avec le symbole " **@** " dans les outils DNS de votre bureau d'enregistrement.

4. Vous devez ensuite fournir un nom d'hôte canonique, qui correspond au domaine **cloudapp.net** de votre application dans le cas présent.

Par exemple, l'enregistrement CNAME suivant renvoie tout le trafic de **www.contoso.com** vers **contoso.cloudapp.net**, le nom de domaine personnalisé de votre application déployée :

<table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
<tr>
<td><strong>Alias/Nom d'hôte/Sous-domaine</strong></td>
<td><strong>Domaine canonique</strong></td>
</tr>
<tr>
<td>www</td>
<td>contoso.cloudapp.net</td>
</tr>
</table>

Un visiteur accédant à **www.contoso.com** ne verra jamais l'hôte réel
(contoso.cloudapp.net), le processus de transfert est donc transparent pour
l'utilisateur final.

> [AZURE.NOTE]
> L'exemple ci-dessus s'applique uniquement au trafic sur le sous-domaine <strong>www</strong>. Puisqu'il n'est pas possible d'utiliser des caractères génériques pour les enregistrements CNAME, vous devez créer un enregistrement CNAME pour chaque domaine/sous-domaine. Pour diriger le trafic de sous-domaines tels que *.contoso.com vers votre adresse cloudapp.net, vous pouvez configurer une entrée de <strong>redirection d'URL</strong> ou de <strong>transfert d'URL</strong> dans vos paramètres DNS. Vous pouvez également créer un enregistrement A.


## Ajouter un enregistrement A pour votre domaine personnalisé

Pour créer un enregistrement A, vous devez tout d'abord connaître l'adresse IP virtuelle de votre service cloud. Ajoutez ensuite une entrée dans la table DNS de votre domaine personnalisé à l'aide des outils fournis par votre bureau d'enregistrement. Chaque bureau d'enregistrement possède sa propre méthode de spécification des enregistrements A, même si le fonctionnement général reste souvent similaire.

1. Utilisez l'une des méthodes suivantes pour obtenir l'adresse IP de votre service cloud.

  * Connectez-vous au [portail de gestion Azure], sélectionnez votre service cloud, puis sélectionnez **Tableau de bord** et recherchez l'entrée ** 	Adresse IP virtuelle (VIP) publique** dans la section **Aperçu rapide**.

   		 ![section Aperçu rapide indiquant l'adresse IP virtuelle][vip]

  * Installez et configurez [Azure Powershell](../articles/install-configure-powershell.md), puis utilisez la commande suivante :

      get-azurevm -servicename votre_nom_service | get-azureendpoint -VM {$_.VM} | select Vip

    Si plusieurs points de terminaison sont associés à votre service cloud, vous recevrez plusieurs lignes contenant l'adresse IP, mais toutes doivent comporter la même adresse.

  Enregistrez l'adresse IP. Vous en aurez besoin lors de la création d'un enregistrement A.

1.  Connectez-vous au site web du bureau d'enregistrement de votre DNS et accédez à la page de gestion DNS. Recherchez des liens ou des zones indiquant **Nom de domaine**, **DNS** ou **Name Server Management**.

2.  Maintenant, cherchez où vous pouvez sélectionner ou saisir vos enregistrements A. Il se peut que vous deviez sélectionner le type d'enregistrement dans une liste déroulante ou accéder à une page de paramètres avancés.

3. Sélectionnez ou entrez le domaine ou sous-domaine qui utilisera cet enregistrement A. Par exemple, sélectionnez **www** si vous souhaitez créer un alias pour **www.domainepersonnalisé.com**. Pour créer une entrée avec des caractères génériques pour l'ensemble des sous-domaines, entrez " __*__ ". Ceci permet de couvrir tous les sous-domaines tels que **mail.customdomain.com**, **login.customdomain.com** et **www.customdomain.com**.

  Si vous souhaitez créer un enregistrement A pour le domaine racine, l'entrée correspondante devrait être répertoriée avec le symbole " **@** " dans les outils DNS de votre bureau d'enregistrement.

4. Entrez l'adresse IP de votre service cloud dans le champ prévu à cet effet. Cette opération permet d'associer le domaine de l'enregistrement A avec l'adresse IP de votre déploiement de service cloud.

Par exemple, l'enregistrement A suivant transfère tout le trafic de **contoso.com** vers **137.135.70.239**, l'adresse IP de votre application déployée :

<table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
<tr>
<td><strong>Nom d'hôte/Sous-domaine</strong></td>
<td><strong>Adresse IP</strong></td>
</tr>
<tr>
<td>@</td>
<td>137.135.70.239</td>
</tr>
</table>

Cet exemple montre comment créer un enregistrement A pour le domaine racine. Pour créer une entrée avec des caractères génériques qui couvre l'ensemble des sous-domaines, entrez " __*__ " comme sous-domaine.

## Étapes suivantes

-   [Gestion des services cloud](../articles/cloud-services-how-to-manage.md)
-   [Mappage du contenu CDN à un domaine personnalisé][]

  [Exposer votre application sur un domaine personnalisé]: #access-app
  [Ajouter un enregistrement CNAME pour votre domaine personnalisé]: #add-cname
  [Exposer vos données sur un domaine personnalisé]: #access-data
  [Échanges d'adresses IP virtuelles]: http://msdn.microsoft.com/library/ee517253.aspx
  [Créer un enregistrement CNAME associant le sous-domaine au compte de stockage]: #create-cname
  [Portail de gestion Azure]: https://manage.windowsazure.com
  [Boîte de dialogue Validation du domaine personnalisé]: http://i.msdn.microsoft.com/dynimg/IC544437.jpg
  [Mappage du contenu CDN à un domaine personnalisé]: http://msdn.microsoft.com/library/windowsazure/gg680307.aspx
  [vip]: ./media/custom-dns/csvip.png
  [csurl]: ./media/custom-dns/csurl.png

<!--HONumber=49-->