<properties
	pageTitle="Profils de score (API REST Azure Search version 2015-02-28-Preview) | Microsoft Azure | API Azure Search (version préliminaire)"
	description="Azure Search est un service de recherche cloud hébergé qui prend en charge le paramétrage des résultats classés en fonction des profils de score définis par l'utilisateur."
	services="search"
	documentationCenter=""
	authors="HeidiSteen"
	manager="jhubbard"
	editor=""/>

<tags
	ms.service="search"
	ms.devlang="rest-api"
	ms.workload="search"
	ms.topic="article"
	ms.tgt_pltfrm="na"
	ms.author="heidist"
	ms.date="08/29/2016" />

# Profils de score (API REST Azure Search Version 2015-02-28-Preview)

> [AZURE.NOTE] Cet article décrit des profils de calcul de score dans la version [2015-02-28-Preview](search-api-2015-02-28-preview.md). Il n’existe actuellement aucune différence entre la version `2015-02-28` documentée sur [MSDN](http://msdn.microsoft.com/library/azure/mt183328.aspx) et la version `2015-02-28-Preview` décrite ici, mais nous offrons quand même ce document en guise d’information pour l’API.

## Vue d'ensemble

Le calcul de score consiste à calculer un score de recherche pour chaque élément renvoyé dans des résultats de recherche. Le score est un indicateur de la pertinence d'un élément dans le contexte de l'opération de recherche en cours. Plus le score est élevé, plus l'élément est pertinent. Dans des résultats de recherche, les éléments sont classés par ordre décroissant de pertinence, sur la base des résultats de recherche calculés pour chacun d'eux.

Le service Azure Search utilise un système de calcul par défaut pour générer les scores, mais vous pouvez personnaliser le mode de calcul à l'aide d'un profil de score. Les profils de score vous permettent de mieux contrôler le classement d'éléments dans des résultats de recherche. Par exemple, vous pouvez privilégier des éléments en fonction de leur revenu potentiel, promouvoir des éléments plus récents, voire en favoriser d'autres restés trop longtemps en stock.

Un profil de score fait partie de la définition d'index, composée de champs, fonctions et paramètres.

Pour vous donner une idée de l'apparence d'un profil de score, l'exemple suivant présente un simple profil « géographique ». Celui-ci privilégie les éléments dont le champ `hotelName` contient le terme recherché. Il utilise également la fonction `distance` pour favoriser les éléments qui se situent dans un rayon de dix kilomètres par rapport à l'emplacement actuel. Si quelqu'un effectue une recherche sur le terme « inn », les documents d'hôtels dont le nom contient cette chaîne de caractères apparaissent en tête des résultats de la recherche.

    "scoringProfiles": [
      {
        "name": "geo",
        "text": {
          "weights": { "hotelName": 5 }
        },
        "functions": [
          {
            "type": "distance",
            "boost": 5,
            "fieldName": "location",
            "interpolation": "logarithmic",
            "distance": {
              "referencePointParameter": "currentLocation",
              "boostingDistance": 10
            }
          }
        ]
      }
    ]

Pour utiliser ce profil de score, votre requête est formulée de façon à spécifier le profil de la chaîne de caractères de requête. Dans la requête ci-dessous, notez la présence du paramètre de requête `scoringProfile=geo`.

    GET /indexes/hotels/docs?search=inn&scoringProfile=geo&scoringParameter=currentLocation--122.123,44.77233&api-version=2015-02-28-Preview

Cette requête effectue une recherche du terme « inn », puis transmet l'emplacement actuel. Notez que cette requête inclut d'autres paramètres, tel que `scoringParameter`. Les paramètres de requête sont décrits dans [Recherche dans des documents (API Azure Search)](search-api-2015-02-28-preview.md#SearchDocs).

Pour voir un exemple plus détaillé de profil de score, cliquez sur [Exemple](#example)

## Qu'est-ce qu'un calcul de score par défaut ?

Un calcul de score détermine un score de recherche pour chaque élément dans un jeu de résultats classés par rang. Un score de recherche est attribué à chaque élément d'un jeu de résultats de recherche. Ils sont ensuite classés du rang le plus élevé au rang le plus bas. Les éléments dont les scores sont plus élevés sont renvoyés à l'application. Par défaut, il s'agit des 50 premiers éléments, mais le paramètre `$top` vous permet de définir le renvoi d'un nombre supérieur ou inférieur d'éléments (jusqu'à 1 000 par réponse).

Par défaut, un score de recherche est calculé sur la base de propriétés statistiques des données et de la requête. Azure Search trouve les documents qui contiennent (en totalité ou en partie, selon le `searchMode`) les termes recherchés indiqués dans la chaîne de requête, en favorisant les documents qui contiennent de nombreuses occurrences du terme recherché. Le score de recherche augmente davantage si le terme est rare dans le corpus de données, mais courant au sein du document. La base de cette approche de la pertinence du calcul est appelée TF-IDF (Term Frequency-Inverse Document Frequency, fréquence de terme-fréquence inverse de document).

En supposant qu'il n'y a pas de tri personnalisé, les résultats sont classés par score de recherche avant d'être renvoyés à l'application appelante. Si la valeur `$top` n'est pas spécifiée, les 50 éléments dont le score de recherche est le plus élevé sont renvoyés.

Des valeurs de score de recherche peuvent être répétées dans un jeu de résultats. Par exemple, vous pouvez avoir dix éléments dont le score est 1,2, vingt éléments dont le score est 1,0, et vingt éléments dont le score est 0,5. Quand plusieurs correspondances ont le même score de recherche, le classement des éléments ayant le même score n'est ni défini ni stable. Si vous exécutez de nouveau la requête, il se peut que des éléments changent de position. Si deux éléments ont un score identique, il est impossible de prédire celui qui apparaîtra en première position.

## Quand utiliser un calcul de score personnalisé

Quand le classement par défaut produit des résultats trop éloignés de vos objectifs, vous devez créer un ou plusieurs profils de calcul de score. Par exemple, vous pouvez décider que la pertinence de la recherche doit privilégier les éléments récemment ajoutés. De même, il se peut qu'un champ affiche une marge bénéficiaire, ou un autre un revenu potentiel. La volonté de privilégier les correspondances qui génèrent des bénéfices pour votre entreprise peut être un facteur important dans la décision d'utiliser des profils de calcul de score.

Un classement basé sur la pertinence peut également être mis en œuvre par l'intermédiaire de profils de calcul de score. Songez aux pages de résultats de recherche que vous avez utilisées par le passé, qui vous permettaient de trier par prix, date, évaluation ou pertinence. Dans Azure Search, les profils de calcul de score déterminent l'option « pertinence ». Vous contrôlez la définition de la pertinence en fonction de vos objectifs et du type d'expérience de recherche que vous souhaitez.

<a name="example"></a>
## Exemple

Comme indiqué précédemment, un calcul de score personnalisé est mis en œuvre à l'aide d'un ou de plusieurs profils de calcul de score définis dans un schéma d'index.

Cet exemple montre le schéma d'un index comprenant deux profils de calcul de score (`boostGenre`, `newAndHighlyRated`). Toute requête sur cet index qui comprend un profil comme paramètre de requête utilise le profil pour évaluer le jeu de résultats.

[Essayez l'exemple suivant](search-get-started-scoring-profiles.md).

    {
      "name": "musicstoreindex",
	  "fields": [
	    { "name": "key", "type": "Edm.String", "key": true },
	    { "name": "albumTitle", "type": "Edm.String" },
	    { "name": "albumUrl", "type": "Edm.String", "filterable": false },
	    { "name": "genre", "type": "Edm.String" },
	    { "name": "genreDescription", "type": "Edm.String", "filterable": false },
	    { "name": "artistName", "type": "Edm.String" },
	    { "name": "orderableOnline", "type": "Edm.Boolean" },
	    { "name": "rating", "type": "Edm.Int32" },
	    { "name": "tags", "type": "Collection(Edm.String)" },
	    { "name": "price", "type": "Edm.Double", "filterable": false },
	    { "name": "margin", "type": "Edm.Int32", "retrievable": false },
	    { "name": "inventory", "type": "Edm.Int32" },
	    { "name": "lastUpdated", "type": "Edm.DateTimeOffset" }
	  ],
      "scoringProfiles": [
        {
	      "name": "boostGenre",
          "text": {
            "weights": {
              "albumTitle": 1.5,
              "genre": 5,
              "artistName": 2
            }
          }
	    },
        {
	      "name": "newAndHighlyRated",
          "functions": [
            {
              "type": "freshness",
              "fieldName": "lastUpdated",
              "boost": 10,
              "interpolation": "quadratic",
              "freshness": {
                "boostingDuration": "P365D"
              }
            },
            {
              "type": "magnitude",
              "fieldName": "rating",
              "boost": 10,
              "interpolation": "linear",
              "magnitude": {
                "boostingRangeStart": 1,
                "boostingRangeEnd": 5,
                "constantBoostBeyondRange": false
              }
            }
          ]
        }
      ],
      "suggesters": [
        {
          "name": "sg",
          "searchMode": "analyzingInfixMatching",
          "sourceFields": ["albumTitle", "artistName"]
        }
      ]
    }


## Workflow

Pour mettre en œuvre un calcul de score personnalisé, ajoutez un profil de calcul de score au schéma qui définit l'index. Vous pouvez avoir plusieurs profils de calcul de score au sein d'un index, mais vous ne pouvez spécifier qu'un seul profil à la fois dans une requête donnée.

Commencez par le [Modèle](#bkmk_template) fourni dans cette rubrique.

Donnez-lui un nom. Les profils de calcul de score sont facultatifs mais, si vous en ajoutez un, le nom est obligatoire. Veillez à respecter les conventions d'affectation des noms pour les champs (commencer par une lettre, en évitant les caractères spéciaux et les mots réservés). Pour en savoir plus sur les règles d'affectation des noms, consultez [Règles d'affectation des noms](http://msdn.microsoft.com/library/azure/dn857353.aspx).

Le corps du profil de calcul de score est construit à partir de champs et de fonctions pondérés.

### Weights ###

La propriété `weights` d’un profil de score spécifie les paires nom-valeur qui affectent une pondération relative à un champ. Dans l’[exemple](#example), les valeurs de pondération des champs albumTitle, genre et artistName sont respectivement 1,5, 5 et 2. Pourquoi la pondération du champ genre est-elle beaucoup plus élevée que celle des autres champs ? Si la recherche est effectuée sur des données relativement homogènes (comme c'est le cas du « genre » dans le `musicstoreindex`), il se peut que vous ayez besoin d'une variance plus importante dans les pondérations relatives. Par exemple, dans le `musicstoreindex`, « rock » apparaît à la fois comme genre et dans des descriptions de genre formulées de façon identique. Si vous souhaitez que le genre ait une pondération plus élevée que la description du genre, la pondération relative du champ Genre doit être sensiblement plus importante.

### Fonctions ###

Les fonctions sont utilisées quand des calculs supplémentaires sont nécessaires dans des contextes spécifiques. Les types de fonction valides sont `freshness`, `magnitude`, `distance` et `tag`. Chaque fonction a des paramètres qui sont uniques à cette dernière.

  - La fonction `freshness` doit être utilisée pour privilégier des résultats en fonction de la nouveauté ou de l’ancienneté d’un élément. Cette fonction peut être utilisée uniquement avec des champs datetime (`Edm.DataTimeOffset`). Notez que l’attribut `boostingDuration` est utilisé uniquement avec la fonction « freshness ».
  - La fonction `magnitude` doit être utilisée pour privilégier des résultats en fonction de l’importance ou de la faiblesse d’une valeur numérique. Parmi les scénarios qui appellent cette fonction figurent la valorisation de la marge bénéficiaire, du prix le plus élevé, du prix le plus bas ou du nombre de téléchargements. Vous pouvez inverser la plage (de la valeur la plus élevée à la valeur la plus basse) si vous souhaitez inverser le schéma (par exemple, pour privilégier des éléments dont le prix est plus bas par rapport aux éléments dont le prix est plus élevé). Dans une gamme de prix allant de $100USD à $1USD, vous devez définir `boostingRangeStart` sur 100 et `boostingRangeEnd` sur 1 pour privilégier les éléments dont le prix est plus bas. Cette fonction peut être utilisée uniquement avec des champs double et integer.
  - La fonction `distance` doit être utilisée pour privilégier des résultats en fonction de la proximité ou de l’emplacement géographique. Cette fonction peut être utilisée uniquement avec des champs `Edm.GeographyPoint`.
  - La fonction `tag` doit être utilisée pour privilégier des résultats en fonction de balises communes entre des documents et des requêtes de recherche. Cette fonction peut être utilisée uniquement avec les champs `Edm.String` et `Collection(Edm.String)`.
  
#### Règles d'utilisation des fonctions ####

  - Le type de fonction (freshness, magnitude, distance, tag) doit être en lettres minuscules.
  - Les fonctions ne peuvent pas contenir de valeurs null ou vides. En particulier, si vous incluez la valeur fieldname, vous devez la spécifier.
  - Les fonctions ne peuvent être appliquées qu'à des champs filtrables. Pour plus d'informations sur les champs filtrables, consultez [Création d'index](search-api-2015-02-28.md#createindex).
  - Vous ne pouvez pas appliquer de fonctions à des champs définis dans la collection de champs d'un index.

Une fois l'index défini, générez-le en chargeant le schéma d'index, puis des documents. Pour obtenir des instructions sur ces opérations, consultez [Création d'index](search-api-2015-02-28-preview.md#createindex) et [Ajout, mise à jour ou suppression de documents](search-api-2015-02-28-preview.md#AddOrUpdateDocuments). Une fois l'index généré, vous disposez d'un profil de calcul de score fonctionnel qui opère avec vos données de recherche.

<a name="bkmk_template"></a>
## Modèle
Cette section présente la syntaxe et le modèle de profils de calcul de score. Pour obtenir la description des attributs, consultez [Référence des attributs d'index](#bkmk_indexref) dans la section suivante.

    ...
    "scoringProfiles": [
      {
        "name": "name of scoring profile",
        "text": (optional, only applies to searchable fields) {
          "weights": {
            "searchable_field_name": relative_weight_value (positive #'s),
            ...
          }
        },
        "functions": (optional) [
          {
            "type": "magnitude | freshness | distance | tag",
            "boost": # (positive number used as multiplier for raw score != 1),
            "fieldName": "...",
            "interpolation": "constant | linear (default) | quadratic | logarithmic",

            "magnitude": {
              "boostingRangeStart": #,
              "boostingRangeEnd": #,
              "constantBoostBeyondRange": true | false (default)
            }

            // (- or -)

            "freshness": {
              "boostingDuration": "..." (value representing timespan over which boosting occurs)
            }

            // (- or -)

            "distance": {
              "referencePointParameter": "...", (parameter to be passed in queries to use as reference location)
              "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
            }

            // (- or -)

            "tag": {
              "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field)
            }
          }
        ],
        "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
      }
    ],
    "defaultScoringProfile": (optional) "...",
    ...

<a name="bkmk_indexref"></a>
## Référence de propriété des profils de score

**Remarque** Vous pouvez appliquer une fonction de calcul de score uniquement à des champs filtrables.

| Propriété | Description |
|----------|-------------|
| `name` | Obligatoire. Nom du profil de calcul de score. Il suit les conventions d'affectation de noms applicables aux champs. Il doit commencer par une lettre, ne peut pas contenir les signes point, deux-points ou @, et ne peut pas commencer par l'expression « azureSearch » (avec respect de la casse). |
| `text` | Contient la propriété Weights |
| `weights` | facultatif. Paire nom-valeur spécifiant un nom de champ et une pondération relative. La pondération relative doit être un entier positif ou un nombre à virgule flottante. Vous pouvez spécifier le nom du champ sans pondération correspondante. Les pondérations permettent d'indiquer l'importance d'un champ par rapport à d'autres. |
| `functions` | facultatif. Notez que vous pouvez appliquer une fonction de calcul de score uniquement à des champs filtrables. |
| `type` | Obligatoire pour les fonctions de calcul de score. Indique le type de fonction à utiliser. Les valeurs autorisées sont `magnitude`, `freshness`, `distance` et `tag`. Vous pouvez inclure plusieurs fonctions dans chaque profil de calcul de score. Le nom de fonction doit être en lettres minuscules. |
| `boost` | Obligatoire pour les fonctions de calcul de score. Nombre positif utilisé comme multiplicateur pour le score brut. Il ne peut pas être égal à 1. |
| `fieldName` | Obligatoire pour les fonctions de calcul de score. Vous pouvez appliquer une fonction de calcul de score uniquement à des champs faisant partie de la collection de champs de l'index, et qui sont filtrables. De plus, chaque type de fonction fait l'objet de restrictions supplémentaires (la valeur freshness est utilisée avec des champs datetime, la valeur amplitude avec des champs integer ou double, la valeur distance avec des champs location et la valeur tag avec des champs string ou string collection). Vous pouvez spécifier un seul champ par définition de fonction. Par exemple, pour utiliser une valeur magnitude deux fois dans le même profil, vous devez inclure deux définitions de magnitude, une pour chaque champ. |
| `interpolation` | Obligatoire pour les fonctions de calcul de score. Définit la pente pour laquelle la valorisation du score augmente, du début à la fin de la plage. Les valeurs autorisées sont `linear` (par défaut), `constant`, `quadratic` et `logarithmic`. Consultez la rubrique [Définition d’interpolations](#bkmk_interpolation) pour plus d’informations. |
| `magnitude` | La fonction de calcul de score magnitude est utilisée pour modifier des classements en fonction de la plage de valeurs pour un champ numérique. Voici quelques exemples d’utilisation courants :<ul><li>Évaluations : modifiez le calcul de score sur la base de la valeur figurant dans le champ « Évaluation ». Lorsque deux éléments sont pertinents, l’élément associé à l’évaluation la plus élevée s’affiche en premier.</li><li>Marge : lorsque deux documents sont pertinents, un détaillant peut préférer privilégier en priorité le document présentant les marges les plus élevées.</li><li>Nombres de clics : pour les applications qui effectuent un suivi du nombre de clics sur les produits ou les pages, vous pouvez utiliser la fonction de magnitude pour privilégier les éléments qui drainent le plus de trafic.</li><li>Nombres de téléchargements : pour les applications qui suivent les téléchargements, la fonction de magnitude permet de privilégier les éléments qui ont le plus de téléchargements.</li></ul> |
| `magnitude:boostingRangeStart` | Définit la valeur de début de la plage sur la base de laquelle les scores de magnitude sont calculés. La valeur doit être un entier ou un nombre à virgule flottante. Pour des évaluations de 1 à 4, il s'agit de 1. Pour des marges de plus de 50 %, il s'agit de 50. |
| `magnitude:boostingRangeEnd` | Définit la valeur de fin de la plage sur laquelle les scores de magnitude sont calculés. La valeur doit être un entier ou un nombre à virgule flottante. Pour les évaluations de 1 à 4, il s'agit de 4. |
| `magnitude:constantBoostBeyondRange` | Les valeurs autorisées sont true ou false (par défaut). Quand la valeur est true, la valorisation complète continue de s'appliquer aux documents dont la valeur pour le champ cible est supérieure à la limite supérieure de la plage. Quand la valeur est false, la valorisation de cette fonction ne s'applique pas aux documents dont la valeur pour le champ cible se situe en dehors de la plage. |
| `freshness` | La fonction de calcul de score freshness permet de modifier les scores de classement d'éléments en fonction des valeurs des champs DateTimeOffset. Par exemple, un élément dont la date est plus récente peut être classé plus haut que des éléments plus anciens. (Notez qu’il est également possible de classer les éléments tels que les événements de calendrier comportant des dates futures afin que les éléments plus proches de la date du jour soient classés plus haut que les éléments plus éloignés dans le temps). Dans la version de service actuelle, une extrémité de la plage doit être définie sur l'heure réelle. L’autre extrémité est une heure dans le passé selon l’attribut `boostingDuration`. Pour privilégier une plage d’heures à venir, utilisez un attribut `boostingDuration` négatif. La vitesse à laquelle la valorisation passe d'une plage maximale à une plage minimale est déterminée par l'interpolation appliquée au profil de calcul de score (voir la figure ci-dessous). Pour inverser le facteur de valorisation appliqué, choisissez un facteur inférieur à 1. |
| `freshness:boostingDuration` | Définit une période d'expiration après laquelle la valorisation s'arrête pour un document spécifique. Pour en savoir plus sur la syntaxe et découvrir des exemples, consultez [Set boostingDuration][#bkmk\_boostdur] dans la section suivante. |
| `distance` | La fonction de calcul de score à distance est utilisée pour affecter le score de documents sur la base de leur proximité ou de l'éloignement par rapport à un emplacement géographique de référence. L’emplacement de référence est indiqué comme partie intégrante de la requête dans un paramètre (à l’aide du paramètre de requête `scoringParameter`) en tant qu’argument lon,lat. |
| `distance:referencePointParameter` | Paramètre à transmettre dans des requêtes, à utiliser comme emplacement de référence. scoringParameter est un paramètre de requête. Pour obtenir une description des paramètres de requête, consultez [Rechercher des documents](search-api-2015-02-28-preview.md#SearchDocs). |
| `distance:boostingDistance` | Nombre indiquant la distance en kilomètres par rapport à l'emplacement de référence où la valorisation se termine. |
| `tag` | La fonction de calcul de score de balises est utilisée pour affecter le score de documents sur la base de balises dans des documents et des requêtes de recherche. Les documents contenant des balises communes avec la requête de recherche seront privilégiés. Les balises pour la requête de recherche sont fournies en tant que paramètre de calcul de score dans chaque requête de recherche (à l’aide du paramètre de requête `scoringParameter`). |
| `tag:tagsParameter` | Paramètre à transmettre dans des requêtes pour spécifier des balises pour une requête spécifique. `scoringParameter` est un paramètre de requête. Pour obtenir une description des paramètres de requête, consultez [Recherche dans des documents](search-api-2015-02-28-preview.md#SearchDocs). |
| `functionAggregation` | facultatif. S'applique uniquement quand des fonctions sont spécifiées. Les valeurs autorisées sont `sum` (par défaut), `average`, `minimum`, `maximum` et `firstMatching`. Un score de recherche est une valeur unique calculée à partir de plusieurs variables, notamment plusieurs fonctions. Cet attribut indique comment les valorisations de toutes les fonctions sont combinées en une valorisation agrégée qui est ensuite appliquée au score du document de base. Le score de base dépend de la valeur tf-idf calculée à partir du document et de la requête de recherche. |
| `defaultScoringProfile` | Lors de l'exécution d'une demande de recherche, le calcul de score par défaut est utilisé (tf-idf uniquement) si aucun profil de calcul de score n'est spécifié. Un nom de profil de calcul de score par défaut peut être défini ici de façon à ce qu'Azure Search utilise ce profil quand aucun profil spécifique n'est fourni dans la requête de recherche. |

<a name="bkmk_interpolation"></a>
## Définition d'interpolations

Les interpolations permettent de définir la pente pour laquelle le score augmente, du début à la fin de la plage. Les interpolations utilisables sont les suivantes :

  - `Linear` : pour les éléments qui s’inscrivent dans la plage de max à min, la valorisation appliquée décroît de manière constante. L'interpolation de type Linear est l'interpolation par défaut pour un profil de calcul de score.
  - `Constant` : pour les éléments qui s’inscrivent dans la plage du début à la fin, une valorisation constante est appliquée aux résultats du classement.
  - `Quadratic` : par rapport à une interpolation de type Linear dont la valorisation décroît de façon constante, une interpolation de type Quadratic décroît initialement plus lentement, puis, lorsqu’elle approche de la plage de fin, beaucoup plus rapidement. Cette option d'interpolation n'est pas autorisée dans les fonctions de calcul de score de balises.
  - `Logarithmic` : par rapport à une interpolation de type Linear dont la valorisation décroît de façon constante, une interpolation de type Logarithmic décroît initialement plus rapidement, puis, lorsqu’elle approche de la plage de fin, beaucoup plus lentement. Cette option d'interpolation n'est pas autorisée dans les fonctions de calcul de score de balises.

<a name="Figure1"></a> ![][1]

<a name="bkmk_boostdur"></a>
## Définition de boostingDuration

`boostingDuration` est un attribut de la fonction freshness. Il permet de définir une période d'expiration après laquelle la valorisation s'arrête pour un document spécifique. Par exemple, pour valoriser une ligne de produits ou une marque pendant une période promotionnelle de 10 jours, vous spécifiez la période de 10 jours en tant que « P10D » pour les documents correspondants. Ou pour valoriser des événements qui vont se produire au cours de la semaine à venir, spécifiez « -P7D ».

La valeur `boostingDuration` doit être au format « dayTimeDuration » XSD (sous-ensemble limité d'une valeur de durée ISO 8601). Le modèle est le suivant : `[-]P[nD][T[nH][nM][nS]]`.

Le tableau suivant fournit plusieurs exemples.

| Durée | boostingDuration |
|----------|------------------|
| 1 jour | « P1D » |
| 2 jours et 12 heures | « P2DT12H » |
| 15 minutes | « PT15M » |
| 30 jours, 5 heures 10 minutes, et 6 334 secondes | « P30DT5H10M6.334S » |

Pour plus d'exemples, consultez [Schéma XML : types de données (site Web W3.org)](http://www.w3.org/TR/xmlschema11-2/).

**Voir aussi** [API REST du service Azure Search](http://msdn.microsoft.com/library/azure/dn798935.aspx) sur MSDN <br/> [Création d’index (Azure Search API)](http://msdn.microsoft.com/library/azure/dn798941.aspx) sur MSDN<br/> [Ajouter un profil de score à un index de recherche](http://msdn.microsoft.com/library/azure/dn798928.aspx) sur MSDN<br/>

<!--Image references-->
[1]: ./media/search-api-scoring-profiles-2015-02-28-Preview/scoring_interpolations.png

<!---HONumber=AcomDC_0914_2016-->