### Vérification de votre connexion à l’aide de PowerShell

Vous pouvez vérifier que votre connexion a réussi à l’aide de l’applet de commande `Get-AzureRmVirtualNetworkGatewayConnection`, avec ou sans `-Debug`.

1. Utilisez l’exemple d’applet de commande suivant, en configurant les valeurs sur les vôtres. Si vous y êtes invité, sélectionnez A pour exécuter tout. Dans l’exemple, `-Name` fait référence au nom de la connexion que vous avez créé et voulez tester.

		Get-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnection -ResourceGroupName MyRG

2. Une fois l’applet de commande exécutée, affichez les valeurs. Dans l’exemple ci-dessous, l’état de la connexion indique « Connecté » et vous pouvez voir les octets d’entrée et de sortie.

		Body:
		{
		  "name": "MyGWConnection",
		  "id":
		"/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/connections/MyGWConnection",
		  "properties": {
		    "provisioningState": "Succeeded",
		    "resourceGuid": "1c484f82-23ec-47e2-8cd8-231107450446b",
		    "virtualNetworkGateway1": {
		      "id":
		"/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/virtualNetworkGa
		teways/vnetgw1"
		    },
		    "localNetworkGateway2": {
		      "id":
		"/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/localNetworkGate
		ways/LocalSite"
		    },
		    "connectionType": "IPsec",
		    "routingWeight": 10,
		    "sharedKey": "abc123",
		    "connectionStatus": "Connected",
		    "ingressBytesTransferred": 33509044,
		    "egressBytesTransferred": 4142431
		  }

### Vérification de votre connexion à l’aide du portail Azure

Dans le portail Azure, vous pouvez afficher l’état de la connexion en accédant à cette dernière. Pour ce faire, plusieurs options s’offrent à vous. Voici un moyen d’accéder à votre connexion.

1. Dans le [portail Azure](http://portal.azure.com), accédez à **Passerelles de réseau virtuel**. Cliquez sur le nom de votre passerelle.
2. Dans le volet, sous **Paramètres**, cliquez sur **Connexions**. Vous pouvez voir l’état de chaque connexion.
3. Pour afficher des informations supplémentaires sur une connexion, cliquez sur son nom. Dans la page Essentials de votre connexion, observez le champ **État**. Ce champ indique « Opération réussie » et « Connecté » lorsqu’une connexion a été établie. Pour connaître le volume de données transitant par la connexion, vous pouvez consulter les champs **Données entrantes** et **Données sortantes**.

	Dans l’exemple ci-dessous, le champ **État** de la connexion indique « Non connecté ».

	![Vérifier la connexion](./media/vpn-gateway-verify-connection-rm-include/connectionverify450.png)

<!---HONumber=AcomDC_0810_2016-->