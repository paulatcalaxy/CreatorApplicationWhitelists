# Creator Application Whitelists


## Introduction

When visting a 3rd party applictation that claims to be selling an asset purported to be from a Creator, Fans must be able to verify the authenticity of that application, and so the asset.

One mechanism for giving Fans this confidence is to allow each Creator to manage and publish an application whitelist of approved applications. Once an application was added to a Creator's whitelist (presuming some offline business deal), Fans considering buying some asset via that application would be able to query the whitelist to determine if the application was approved.

Importantly, it must be possible for an application to be removed from a Creator's whitelist when appropriate - as determined by the Creator.

## Specification

A Creator's 'application whitelist' will manifest as a JSON file on Hedera's file service. It is by creating and updating this file that a Creator will manage their application whitelist.



### Application Whitelist Schema 

The application whitelist is a JSON file listing the application's that have been approved by the Creator.

Each entry in the list stores a name and TLD URL for an application


    [
	    {
		    name: "Shopify",
		    URL: "www.shopify.com"
	    },
	    {
		    name: "AppB",
		    URL: "www.appb.com"
	    },
    ]

The order of entries in the list is not material.

### Binding Whitelist to Creator

It must be possible to establish that a given application whitelist (with a given Hedera File identifier) is associated with a particular Creator (as identified by their social token's token identifier). If not, a malicious application could direct a Fan to a fake application whitelist with its own name on the list - fooling the Fan into thinking the application was valid and approved by the Creator when in fact it wasn't.

The Creator's social token's memo field will be used to point to the appropriate application whitelist. Rather than directly specifying the file identifier within the memo field, the memo field will contain a Decentralized Identifier (DID) that itself 'points' to a JSON DID Document stored off chain (in an HCS appnet run by participants in the Creator's Galaxy). The DID Document will contain the File identifier of the application whitelist.

The sequence by which an application whitelist is resolved is Query Token -> Determine DID from memo -> Resolve DID into DID Document -> Determine application whitelist file identifier -> Retrieve application whitelist JSON

While the DID model introduces an extra lookup, it provides a flexible integration point for the future. As an example, we can leverage the DID mechanism to bind a Verifiable Credential issued to the Creator to the social token in the same manner as for this application whitelist. Additinally, the DID model allows for the application whitelist to be stored 

### DID Document Schema

A DID document contains a set of service endpoints. 

We will extend the schema with a service endpoint of type 'CalaxyApplicationWhitelist'

    {
      "service": [{
        "id":"did:hedera:mainnet:123sjgjhdfughdiufg#application-whitelist",
        "type": "CalaxyApplicationWhitelist", 
        "serviceEndpoint": "0.0.12345"
      }]
    }

The serviceEndpoint carries the Hedera File identifier of the application whitelist.

## Open Issues

How does an application prove it is on a Creator's application whitelist other than giving a Fan the ability to view the whitelist and manually compare the entries to the application's own TLD? Might the application whitelist contain DIDs , and the applications would demonstrate ownerhip of their own DID?

## Security Considerations

??
