# Kudy z nudy API

The following describes the application programming interface (API) for accessing published content and other services on the [Kudy z nudy](https://www.kudyznudy.cz/) portal. The API is used to integrate external entities and applications and allows them to retrieve primary content published on the portal. The same data format is also used for reverse integration - i.e. [importing data from external entities into Kudy z nudy](/Import.md).

The operator of the portal [Kudy z nudy](https://www.kudyznudy.cz) and the API provider is the [CzechTourism](https://www.czechtourism.cz/) agency.

## API access

The API is only available to CzechTourism contractual partners. **To access the API, it is necessary to have an account on the Kudy z nudy portal and to have access rights to this account.** Access rights will be granted to the partner by the API provider. For more information on gaining access, please visit [Kudy z nudy - API](https://www.kudyznudy.cz/faq-casto-kladene-otazky/api).

## Basic information about API

API runs at [https://api.kudyznudy.cz](https://api.kudyznudy.cz/).

The automatically generated API documentation in Swagger format is at [https://api.kudyznudy.cz/swagger/ui/index](https://api.kudyznudy.cz/swagger/ui/index).

The documentation provides examples of API calls using cURL.

## Data format

The individual services described below return data in [JSON](https://www.json.org/) format. The data models for published content were designed following the Open Data model [https://opendata.gov.cz](https://opendata.gov.cz/), or the Open Formal Standards [https://data.gov.cz/ofn/](https://data.gov.cz/ofn/).


## Authentication to API

To call API services, you must log in with a user account that has the appropriate access rights [https://api.kudyznudy.cz/login](https://api.kudyznudy.cz/login). After a successful login, the user is returned a Bearer token, which is needed to sign all calls within the API.

A user may have full or limited access (depending on the level of partnership). See  [Kudy z nudy - API](https://www.kudyznudy.cz/faq-casto-kladene-otazky/api). In the case of full access, the complete data object is always returned. In the case of limited access, only the public part is returned.

## Activity (Tourist destination)

Activity (tourist destination) is a type of content published on the portal Kudy z boredy. It is characterized by an unlimited duration.

Activities can be found in the [Co chcete dělat](https://www.kudyznudy.cz/co-chcete-delat) (What you want to do) section of the portal. .

### List of activities (tourist destinations)

The method returns a list of published activities.

For a list of all activities, call:

    curl --location --request GET 'https://api.kudyznudy.cz/content/activities' \
    --header 'Authorization: Bearer <ACCESS_TOKEN>'

To get activities changed after a certain time, the service call is as follows:

    curl --location --request POST 'https://api.kudyznudy.cz/content/activities' \
    --header 'Content-Type: application/json' \
    --header 'Accept: application/json' \
    --header 'Authorization: Bearer <ACCESS_TOKEN>' \
    --data-raw '{
        "date": "2022-10-10T00:00:00.000Z"
    }'

*The method is useful, for example, for more efficient data synchronization.*

The answer is an array of JSON objects. For example::

    [
        {
            "@context": "https://www.kudyznudy.cz/kzn/context/v1/dokument.jsonld",
            "typ": "Turistický cíl",
            "id": "630586e5-9541-4099-84ef-f8a35dc8b745",
            "iri": "https://www.kudyznudy.cz/aktivity/komentovane-prohlidky-arealu-zirec-a-domova-sv-jos",
            "název": {
                "cs": "Bylinkové zahrady Žíreč – komentované prohlídky v areálu Žireč a Domova sv. Josefa"
            },
            "vytvořeno": {
                "typ": "Časový okamžik",
                "datum_a_čas": "2022-10-10T10:21:06.7089805"
            },
            "aktualizováno": {
                "typ": "Časový okamžik",
                "datum_a_čas": "2022-10-10T11:44:32.4484005"
            }
        }
    ]


| **Value** | **Description** |
| --- | --- |
| @context | Data format description - jsonld template (JSON schema: [https://www.kudyznudy.cz/kzn/context/v1/dokument.jsonld](https://www.kudyznudy.cz/kzn/context/v1/dokument.jsonld)) |
| typ | Turistický cíl (Tourist destination) |
| id | Unique GUID of the record |
| iri | Unique record address (URL of the article on Kudy z nudy) |
| název | Article title (headline) |
| vyvořeno | Date and time of creation |
| aktualizováno | Date and time of last change |

### Activity detail (tourist destination)

Activity detail (tourist destination):

    curl --location --request POST 'https://api.kudyznudy.cz/content/activities' \
    --header 'Content-Type: application/json' \
    --header 'Accept: application/json' \
    --header 'Authorization: Bearer <ACCESS_TOKEN>' \
    --data-raw '{
        "id": "630586e5-9541-4099-84ef-f8a35dc8b745"
    }'

The output is a JSON object. Depending on the access type, the full object or just the public part of the object is returned to the user.

Full-featured object (in full-access mode):

    {
    "@context": "https://www.kudyznudy.cz/kzn/context/v1/turisticky-cil.jsonld",
    "typ": "Turistický cíl",
    "typ_turistického_cíle": [
        "https://www.kudyznudy.cz/co-chcete-delat/pamatky",
        "https://www.kudyznudy.cz/co-chcete-delat/zazitky",
        "https://www.kudyznudy.cz/co-chcete-delat/zivotni-styl"
    ],
    "kategorie_turistického_cíle": [
        "https://www.kudyznudy.cz/co-chcete-delat/pamatky/cirkevni-pamatky",
        "https://www.kudyznudy.cz/co-chcete-delat/zazitky/organizovane-vylety-a-exkurze",
        "https://www.kudyznudy.cz/co-chcete-delat/zazitky/netradicni-prohlidky"
    ],
    "id": "630586e5-9541-4099-84ef-f8a35dc8b745",
    "iri": "https://www.kudyznudy.cz/aktivity/komentovane-prohlidky-arealu-zirec-a-domova-sv-jos",
    "název": {
        "cs": "Bylinkové zahrady Žíreč – komentované prohlídky v areálu Žireč a Domova sv. Josefa"
    },
    "vytvořeno": {
        "typ": "Časový okamžik",
        "datum_a_čas": "2022-10-10T10:21:06.7089805"
    },
    "aktualizováno": {
        "typ": "Časový okamžik",
        "datum_a_čas": "2022-10-10T11:44:32.4484005"
    },
    "popis": {
        "cs": "Zažijte příběh Areálu Žireč. Navštívíte jedinečné cyklomuzeum, kavárnu i bylinkovou zahradu v místech bývalé jezuitské rezidence. Zaposloucháte se do zvuku unikátní zvonkohry a projdete se po místech, kde působili jezuité. Poznáte dávnou historii i nový příběh pomoci lidem s roztroušenou sklerózou."
    },
    "dlouhý_popis": {
        "cs": "<a href=\"~/aktivity/barokni-areal-zirec\">Areál Žireč</a> se nachází v části obce Žireč ve Dvoře Králové nad Labem. Můžete navštívit krásnou bylinkovou zahradu a posedět v ní s šálkem kávy z kavárny Damián nebo se projít v rozlehlém přírodním parku. Areál je zároveň domovem pro lidi nemocné roztroušenou sklerózou mozkomíšní, kteří žijí v Domově sv. Josefa. Hlavní budova Domova je bývalou barokní jezuitskou rezidencí. Svojí návštěvou podpoříte péči o nemocné i lidi se sníženou pracovní schopností, kteří vás mohou obsloužit v jednotlivých provozech. Návštěvy a komentované prohlídky jsou možné domluvit po celý rok.<br />\r\n<br />\r\nV Areálu naleznete největší cykloexpozici ve střední Evropě. V kostele sv. Anny se nalézá unikátní barokní zvonový klavír na světě &ndash; jediný svého druhu, dále vzácné varhany, jeden z mála dochovaných deskových jezuitských betlémů a mnoho dalších zajímavých předmětů."
    },
    "vhodné_pro_děti": false,
    "vhodné_pro_zvířata": false,
    "za_každého_počasí": false,
    "bezbariériový_přístup": true,
    "časová_náročnost": 1.5,
    "registrace": null,
    "pořadatel": {
        "typ": "Osoba",
        "název": {
            "cs": "Areál Žireč, Domov sv. Josefa"
        },
        "geometrie": {
            "typ": "Point",
            "coordinates": [
                50.413444519042969,
                15.853754043579102
            ]
        },
        "adresa": {
            "typ": "Kontaktní adresa",
            "ulice": "Žireč 1",
            "obec": "Dvůr Králové nad Labem",
            "psč": "54404",
            "kontakt": {
                "typ": "Kontakt",
                "email": "mailto:kollova@dsj-zirec.cz",
                "mobil": "tel:+420491610533",
                "facebook": "https://www.facebook.com/arealzirec",
                "instagram": null,
                "twitter": null,
                "url": "https://www.arealzirec.cz"
            }
        },
        "kontaktní_osoba": {
            "typ": "Kontaktní osoba",
            "jméno": "Andrea Kollová",
            "email": "mailto:kollova@dsj-zirec.cz",
            "mobil": "tel:+420730144334"
        }
    },
    "umístění": [],
    "kategorie_vstupného": {
        "typ": "Vstupné",
        "cena": {
            "typ": "Částka",
            "výše": 130.0,
            "měna": "https://publications.europa.eu/resource/authority/currency/CZK"
        },
        "snížená_cena": {
            "typ": "Částka",
            "výše": 90.0,
            "měna": "https://publications.europa.eu/resource/authority/currency/CZK"
        },
        "rodinné_vstupné": null,
        "poznámka": {
            "cs": "Nebo pouze kostel 50/30 či cykloexpozice 90/60 Info a rezervace na 731 604 204"
        }
    },
    "otevírací_doba": null,
    "příloha": [
        {
            "typ": "Digitální objekt",
            "url": "https://dbcsx3kp2k1lc.cloudfront.net/files/a9/a9afa917-20d3-44f7-83be-4abf7f5ceccb.webp?v=20221010111834",
            "autor_díla": null,
            "typ_média": "https://www.iana.org/assignments/media-types/image/webp",
            "název": {
                "cs": "Bylinková zahrada"
            },
            "popis": null
        },
        {
            "typ": "Digitální objekt",
            "url": "https://dbcsx3kp2k1lc.cloudfront.net/files/17/17609ec2-a7bf-46c7-bfe0-5dea754c9365.webp?v=20221010111834",
            "autor_díla": null,
            "typ_média": "https://www.iana.org/assignments/media-types/image/webp",
            "název": {
                "cs": "Zvonový klavír"
            },
            "popis": null
        },
        {
            "typ": "Digitální objekt",
            "url": "https://dbcsx3kp2k1lc.cloudfront.net/files/52/5219812f-c614-4114-a321-2b6b30d288f2.webp?v=20221010111834",
            "autor_díla": null,
            "typ_média": "https://www.iana.org/assignments/media-types/image/webp",
            "název": {
                "cs": "Bylinková zahrada"
            },
            "popis": null
        },
        {
            "typ": "Digitální objekt",
            "url": "https://dbcsx3kp2k1lc.cloudfront.net/files/0b/0b5e7a00-6158-4e2b-9e35-041cb719d7cd.webp?v=20221010111834",
            "autor_díla": null,
            "typ_média": "https://www.iana.org/assignments/media-types/image/webp",
            "název": {
                "cs": "Kavárna Damián"
            },
            "popis": null
        },
        {
            "typ": "Digitální objekt",
            "url": "https://dbcsx3kp2k1lc.cloudfront.net/files/4b/4b9637d3-70d6-4fc7-9e91-2d5dd68f7cd6.webp?v=20221010111834",
            "autor_díla": null,
            "typ_média": "https://www.iana.org/assignments/media-types/image/webp",
            "název": {
                "cs": "Bylinková zahrada"
            },
            "popis": null
        },
        {
            "typ": "Digitální objekt",
            "url": "https://dbcsx3kp2k1lc.cloudfront.net/files/a0/a0791a5c-84f0-416e-b460-cb746baf3871.webp?v=20221010111834",
            "autor_díla": null,
            "typ_média": "https://www.iana.org/assignments/media-types/image/webp",
            "název": {
                "cs": "Expozice cyklistiky"
            },
            "popis": null
        },
        {
            "typ": "Digitální objekt",
            "url": "https://dbcsx3kp2k1lc.cloudfront.net/files/1c/1c2cd7d9-36d8-4a7b-9705-f7b4b8b224e7.webp?v=20221010111834",
            "autor_díla": null,
            "typ_média": "https://www.iana.org/assignments/media-types/image/webp",
            "název": {
                "cs": "Kostel sv. Anny"
            },
            "popis": null
        },
        {
            "typ": "Digitální objekt",
            "url": "https://dbcsx3kp2k1lc.cloudfront.net/files/5f/5ff23727-9b12-46d6-858e-ddd92fc20795.webp?v=20221010111834",
            "autor_díla": null,
            "typ_média": "https://www.iana.org/assignments/media-types/image/webp",
            "název": {
                "cs": "Areál Žireč"
            },
            "popis": null
        }
    ]
}

| **Value** | **Description** |
| --- | --- |
| @context | Data format description - jsonld template (JSON schema: [https://www.kudyznudy.cz/kzn/context/v1/dokument.jsonld](https://www.kudyznudy.cz/kzn/context/v1/dokument.jsonld)) |
| typ | Turistický cíl (Tourist destination) |
| id | Unique GUID of the record |
| iri | Unique record address (URL of the article on Kudy z nudy) |
| název | Article title (headline) |
| vyvořeno | Date and time of creation |
| aktualizováno | Date and time of last change |
| typ_turistického_cíle | Array of tourist destination types (the first one is the main one) |
| kategorie_turistického_cíle | Category field of tourist destination types |
| popis | Short annotation (introduction) - plain text |
| dlouhý_popis | HTML text following the short description |
| vhodné_pro_děti | Suitable for children (true / false) |
| vhodné_pro_zvířata | Suitable for animals (true / false) |
| za_každého_počasí | In any weather (true / false) |
| bezbariériový_přístup | Barrier-free access (true / false) |
| časová_náročnost | Number of hours needed to visit the site |
| registrace | URL address of the registration on the external website |
| pořadatel |  Address and contact to the organizer |
| umístění | Venues (addresses and contacts)) |
| kategorie_vstupného | Prices of individual types of admission |
| otevírací_doba | Definition of opening hours on individual days |
| příloha | Array of images and their metadata |

Public part of the building (in case of limited public access):

    {
    "@context": "https://www.kudyznudy.cz/kzn/context/v1/turisticky-cil-public.jsonld",
    "typ": "Turistický cíl",
    "id": "bb3f60b0-c668-4f31-a13c-780bb1b7c87f",
    "iri": "https://www.kudyznudy.cz/aktivity/poutni-kostel-sv-jana-nepomuckeho-na-zelene-hore",
    "název": {
        "cs": "Zelená hora – poutní kostel sv. Jana Nepomuckého u Žďáru n. S."
    },
    "vytvořeno": {
        "typ": "Časový okamžik",
        "datum_a_čas": "2010-04-11T10:21:57"
    },
    "aktualizováno": {
        "typ": "Časový okamžik",
        "datum_a_čas": "2022-07-14T05:32:48.4597958"
    },
    "popis": {
        "cs": "Kostel sv. Jana Nepomuckého na Zelené hoře ve Žďáru nad Sázavou je národní kulturní památka zapsaná na Seznamu světového dědictví UNESCO. Byl postaven na začátku 20. let 18. století a je nepochybně nejosobitějším dílem J. B. Santiniho Aichla."
        },
    "geometrie": {
        "typ": "Point",
        "coordinates": [
                49.5828245,
                15.9375815
        ]
    },
    "příloha": [
        {
            "typ": "Digitální objekt",
            "url": "https://dbcsx3kp2k1lc.cloudfront.net/files/8b/8b880b32-2c6d-49d1-816b-9e0f3d165fa2.jpg?v=20220714053247",
            "autor_díla": null,
            "typ_média": "https://www.iana.org/assignments/media-types/image/jpeg",
            "název": {
                "cs": "Poutmí kostel ve Ždáru_foto Libor Svácek"
            },
            "popis": null
        },
        {
            "typ": "Digitální objekt",
            "url": "https://dbcsx3kp2k1lc.cloudfront.net/files/f3/f30fbc88-9eb2-4a03-96b3-a69a36bfb79c.jpg?v=20220714053247",
            "autor_díla": null,
            "typ_média": "https://www.iana.org/assignments/media-types/image/jpeg",
            "název": {
                "cs": "Autor Chris Borg"
            },
            "popis": {
                "cs": "Autor Chris Borg"
            }
        }
        ]
    }

| **Value** | **Description** |
| --- | --- |
| @context | Data format description - jsonld template (JSON schema: [https://www.kudyznudy.cz/kzn/context/v1/dokument.jsonld](https://www.kudyznudy.cz/kzn/context/v1/dokument.jsonld)) |
| typ | Turistický cíl (Tourist destination) |
| id | Unique GUID of the record |
| iri | Unique record address (URL of the article on Kudy z nudy) |
| název | Article title (headline) |
| vyvořeno | Date and time of creation |
| aktualizováno | Date and time of last change |
| typ_turistického_cíle | Array of tourist destination types (the first one is the main one) |
| kategorie_turistického_cíle | Category field of tourist destination types |
| popis | Short annotation (introduction) - plain text |
| dlouhý_popis | HTML text following the short description |
| vhodné_pro_děti | Suitable for children (true / false) |
| vhodné_pro_zvířata | Suitable for animals (true / false) |
| za_každého_počasí | In any weather (true / false) |
| bezbariériový_přístup | Barrier-free access (true / false) |
| časová_náročnost | Number of hours needed to visit the site |
| registrace | URL address of the registration on the external website |
| pořadatel |  Address and contact to the organizer |
| umístění | Venues (addresses and contacts)) |
| kategorie_vstupného | Prices of individual types of admission |
| otevírací_doba | Definition of opening hours on individual days |
| příloha | Array of images and their metadata |
| geometrie | GPS coordinates |

## Action (Event)

Action (event) is another type of content published on the portal Kudy z nudy. It is characterized by a limited duration.

Events can be found in the [Calendar of Events](https://www.kudyznudy.cz/kalendar-akci). section of the portal.

### List of actions (events)

The method returns a list of published actions.

For a list of events, call:

    curl --location --request GET 'https://api.kudyznudy.cz/content/events' \
    --header 'Accept: application/json' \
    --header 'Authorization: Bearer <ACCESS_TOKEN>'

Retrieving actions changed after a certain time by calling:

    curl --location --request POST 'https://api.kudyznudy.cz/content/events' \
    --header 'Content-Type: application/json' \
    --header 'Accept: application/json' \
    --header 'Authorization: Bearer <ACCESS_TOKEN>' \
    --data-raw '{
        "date": "2022-10-10T09:23:40.085Z"
    }'

Action list services output - JSON object array:

    [
        {
            "@context": "https://www.kudyznudy.cz/kzn/context/v1/dokument.jsonld",
            "typ": "Událost",
            "id": "e5ac8753-ef5d-478c-96f7-7e804c5ba4d2",
            "iri": "https://www.kudyznudy.cz/akce/tantra-joga-mohendzodaro-zena-kraska-zena-osklivka",
            "název": {
                "cs": "Mohendžodáro – žena kráska – žena ošklivka"
            },
            "vytvořeno": {
                "typ": "Časový okamžik",
                "datum_a_čas": "2022-10-06T19:07:08.1191666"
            },
            "aktualizováno": {
                "typ": "Časový okamžik",
                "datum_a_čas": "2022-10-10T12:37:01.712556"
            }
        },
        {
            "@context": "https://www.kudyznudy.cz/kzn/context/v1/dokument.jsonld",
            "typ": "Událost",
            "id": "bb96c39e-3b0f-4035-877b-befe1b684536",
            "iri": "https://www.kudyznudy.cz/akce/dynovy-podzim-v-trojske-botanicke-zahrade",
            "název": {
                "cs": "Dýňový podzim v trojské botanické zahradě "
            },
            "vytvořeno": {
                "typ": "Časový okamžik",
                "datum_a_čas": "2016-08-24T17:25:36"
            },
            "aktualizováno": {
                "typ": "Časový okamžik",
                "datum_a_čas": "2022-10-10T12:34:16.884373"
            }
        }
    ]

| **Value** | **Description** |
| --- | --- |
| @context | Data format description - jsonld template (JSON schema: [https://www.kudyznudy.cz/kzn/context/v1/dokument.jsonld](https://www.kudyznudy.cz/kzn/context/v1/dokument.jsonld)) |
| typ | Událost (Event) |
| id | Unique GUID of the record |
| iri | Unique record address (URL of the article on Kudy z nudy) |
| název | Article title (headline) |
| vyvořeno | Date and time of creation |
| aktualizováno | Date and time of last change |

### Action (event) detail

Service for getting details of a published event:

    curl --location --request POST 'https://api.kudyznudy.cz/content/events' \
    --header 'Content-Type: application/json' \
    --header 'Accept: application/json' \
    --header 'Authorization: Bearer <ACCESS_TOKEN>' \
    --data-raw '{
        "id": "d3ba097e-9ad7-4de2-af3c-18e395dfbbcc"
    }'

The output of the action detail service is a JSON object. Depending on the access type, the full object or just the public part of the object is returned to the user.

Full-featured object (in full access):

    {
        "@context": "https://www.kudyznudy.cz/kzn/context/v1/udalost.jsonld",
        "typ": "Událost",

    "doba_trvání": [
            {
                "typ": "Časová specifikace",

    "časový_interval": {
                    "typ": "Časový interval",
                    "začátek": {
                        "typ": "Časový okamžik",

    "datum_a_čas": "2022-10-29T10:00:00"
                    },
                    "konec": {
                        "typ": "Časový okamžik",

    "datum_a_čas": "2022-10-30T17:00:00"
                    }
                }
            }
        ],

    "typ_události": [
            "https://www.kudyznudy.cz/kalendar-akci/festivaly",
            "https://www.kudyznudy.cz/kalendar-akci/vystavy"
        ],
        "id": "d3ba097e-9ad7-4de2-af3c-18e395dfbbcc",
        "iri": "https://www.kudyznudy.cz/akce/prague-car-festival-2021",
        "název": {
            "cs": "Prague Car Festival 2022"
        },
        "vytvořeno": {
            "typ": "Časový okamžik",

    "datum_a_čas": "2021-09-21T18:57:59.4463373"
        },
        "aktualizováno": {
            "typ": "Časový okamžik",

    "datum_a_čas": "2022-06-03T02:33:10.3900509"
        },
        "popis": {
            "cs": "Největší výstava ve střední a východní Evropě se zaměřením na tuning, motorsport, historické automobily a motocykly."
        },

    "dlouhý_popis": {
            "cs": "Jedenáctý ročník akce evropského významu nabídne vystavovatelům i návštěvníkům \<strong\>jedinečnou podívanou \</strong\>na široké spektrum unikátních automobilů a motocyklů. Výstava spojuje oblasti upravených a sportovních vozů, světa motorsportu a historie vozidel. \<strong\>Prague Car Festival\</strong\> představí na ploše přes 30 000 m\<sup\>2\</sup\> \<strong\>nejlepší tuning z celé Evropy\</strong\>, českou i zahraniční špičku automobilových a motocyklových závodů a průřez bohatou historií vývoje automobilů a motocyklů \<strong\>od předválečného období až po současnost\</strong\>.\r\n\r\n\<h3\>\<br /\>\r\n\<strong\>BSR Tuning Expo\</strong\>\</h3\>\r\nVýstava BSR Tuning Expo nabízí skvosty z české tuningové scény a celé Evropy, profesionální tuningové firmy, sportovní automobily a americké vozy tvoří hlavní obsahovou část výstavních hal. Návštěvníci uvidí přes 500 jedinečných exponátů. K tomu soutěže, workshopy, akční jízdy a srazy na venkovní ploše a také doprovodný program pro celou rodinu vytvářejí jedinečnou příležitost pro návštěvníky všech generací."
        },

    "vhodné_pro_děti": true,

    "vhodné_pro_zvířata": true,

    "za_každého_počasí": true,

    "bezbariériový_přístup": true,

    "časová_náročnost": 2,
        "registrace": {
            "cs": "https://praguecarfestival.cz/vstupenky/"
        },
        "pořadatel": {
            "typ": "Osoba",
            "název": {
                "cs": "Prague Car Festival s.r.o."
            },
            "geometrie": {
                "typ": "Point",
                "coordinates": [
                    50.12938386099312,
                    14.551111294871998
                ]
            },
            "adresa": {
                "typ": "Kontaktní adresa",
                "ulice": "Hornopočernická 13",
                "obec": "Praha",
                "psč": "19700",
                "kontakt": {
                    "typ": "Kontakt",
                    "email": "mailto:office@praguecarfestival.cz",
                    "mobil": "tel:+420608320336",
                    "facebook": null,
                    "instagram": null,
                    "twitter": null,
                    "url": "https://praguecarfestival.cz/"
                }
            },

    "kontaktní_osoba": {
                "typ": "Kontaktní osoba",
                "jméno": "Martin Apeltauer",
                "email": "mailto:apeltauerm@gmail.com",
                "mobil": "tel:+420608320336"
            }
        },
        "umístění": [
            {
                "typ": "Místo konání",
                "název": {
                    "cs": "PVA EXPO PRAHA Letňany"
                },
                "geometrie": {
                    "typ": "Point",
                    "coordinates": [
                        50.129423083832535,
                        14.514344635522487
                    ]
                },
                "adresa": {
                    "typ": "Kontaktní adresa",
                    "ulice": "Beranových 667",
                    "obec": "Praha 9",
                    "psč": "19900"
                },

    "kontaktní_osoba": null
            }
        ],

    "kategorie_vstupného": {
            "typ": "Vstupné",
            "cena": {
                "typ": "Částka",
                "výše": 300,
                "měna": "https://publications.europa.eu/resource/authority/currency/CZK"
            },

    "snížená_cena": {
                "typ": "Částka",
                "výše": 200,
                "měna": "https://publications.europa.eu/resource/authority/currency/CZK"
            },

    "rodinné_vstupné": null,
            "poznámka": {
                "cs": "Předprodej on-line za zvýhodněnou cenu."
            }
        },

    "otevírací_doba": null,
        "příloha": [
            {
                "typ": "Digitální objekt",
                "url": "https://dbcsx3kp2k1lc.cloudfront.net/files/70/70d1a90e-d5c2-4fd1-becb-c531e3ec65be.jpg?v=20220603023310",

    "autor_díla": null,

    "typ_média": "https://www.iana.org/assignments/media-types/image/jpeg",
                "název": {
                    "cs": "PCF"
                },
                "popis": null
            },
            {
                "typ": "Digitální objekt",
                "url": "https://dbcsx3kp2k1lc.cloudfront.net/files/0f/0ff52200-7302-4553-8412-81a307c00d4b.jpg?v=20220603023310",

    "autor_díla": null,

    "typ_média": "https://www.iana.org/assignments/media-types/image/jpeg",
                "název": {
                    "cs": "PCF"
                },
                "popis": null
            }
        ]
    }

| **Value** | **Description** |
| --- | --- |
| @context | Data format description - jsonld template (JSON schema: [https://www.kudyznudy.cz/kzn/context/v1/dokument.jsonld](https://www.kudyznudy.cz/kzn/context/v1/dokument.jsonld)) |
| typ | Událost (Event) |
| id | Unique GUID of the record |
| iri | Unique record address (URL of the article on Kudy z nudy) |
| název | Article title (headline) |
| vyvořeno | Date and time of creation |
| aktualizováno | Date and time of last change |
| typ_turistického_cíle | Array of tourist destination types (the first one is the main one) |
| kategorie_turistického_cíle | Category field of tourist destination types |
| popis | Short annotation (introduction) - plain text |
| dlouhý_popis | HTML text following the short description |
| vhodné_pro_děti | Suitable for children (true / false) |
| vhodné_pro_zvířata | Suitable for animals (true / false) |
| za_každého_počasí | In any weather (true / false) |
| bezbariériový_přístup | Barrier-free access (true / false) |
| časová_náročnost | Number of hours needed to visit the site |
| registrace | URL address of the registration on the external website |
| pořadatel |  Address and contact to the organizer |
| umístění | Venues (addresses and contacts)) |
| kategorie_vstupného | Prices of individual types of admission |
| otevírací_doba | Definition of opening hours on individual days |
| příloha | Array of images and their metadata |
| geometrie | GPS coordinates |
| doba_trvání | Start and end of the event |
| typ_události | Array of event types (the first one is the main one) |

Public part of the building (in case of limited public access):

    {
        "@context": "https://www.kudyznudy.cz/kzn/context/v1/udalost-public.jsonld",
        "typ": "Událost",
        "doba_trvání": [
            {
                "typ": "Časová specifikace",
                "časový_interval": {
                    "typ": "Časový interval",
                    "začátek": {
                        "typ": "Časový okamžik",
                        "datum_a_čas": "2022-10-29T10:00:00"
                    },
                    "konec": {
                        "typ": "Časový okamžik",
                        "datum_a_čas": "2022-10-30T17:00:00"
                    }
                }
            }
        ],
        "id": "d3ba097e-9ad7-4de2-af3c-18e395dfbbcc",
        "iri": "https://www.kudyznudy.cz/akce/prague-car-festival-2021",
        "název": {
            "cs": "Prague Car Festival 2022"
        },
        "vytvořeno": {
            "typ": "Časový okamžik",
            "datum_a_čas": "2021-09-21T18:57:59.4463373"
        },
        "aktualizováno": {
            "typ": "Časový okamžik",
            "datum_a_čas": "2022-06-03T02:33:10.3900509"
        },
        "popis": {
            "cs": "Největší výstava ve střední a východní Evropě se zaměřením na tuning, motorsport, historické automobily a motocykly."
        },
        "geometrie": {
            "typ": "Point",
            "coordinates": [
                50.12938386099312,
                14.551111294871998
            ]
        },
        "příloha": [
            {
                "typ": "Digitální objekt",
                "url": "https://dbcsx3kp2k1lc.cloudfront.net/files/70/70d1a90e-d5c2-4fd1-becb-c531e3ec65be.jpg?v=20220603023310",
                "autor_díla": null,
                "typ_média": "https://www.iana.org/assignments/media-types/image/jpeg",
                "název": {
                    "cs": "PCF"
                },
                "popis": null
            },
            {
                "typ": "Digitální objekt",
                "url": "https://dbcsx3kp2k1lc.cloudfront.net/files/0f/0ff52200-7302-4553-8412-81a307c00d4b.jpg?v=20220603023310",
                "autor_díla": null,
                "typ_média": "https://www.iana.org/assignments/media-types/image/jpeg",
                "název": {
                    "cs": "PCF"
                },
                "popis": null
            }
        ]
    }

| **Value** | **Description** |
| --- | --- |
| @context | Data format description - jsonld template (JSON schema: [https://www.kudyznudy.cz/kzn/context/v1/dokument.jsonld](https://www.kudyznudy.cz/kzn/context/v1/dokument.jsonld)) |
| typ | Událost (Event) |
| id | Unique GUID of the record |
| iri | Unique record address (URL of the article on Kudy z nudy) |
| název | Article title (headline) |
| vyvořeno | Date and time of creation |
| aktualizováno | Date and time of last change |
| popis | Short annotation (introduction) - plain text |
| příloha | Array of images and their metadata |
| geometrie | GPS coordinates |
| doba_trvání | Start and end of the event |
