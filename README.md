# CreatorWhitelists


## Introduction

When visting a 3rd party applictation that claims to be selling an asset purported to be from a Creator, Fans must be able to verify the authenticity of that application, and so the asset.

One mechanism for giving Fans this confidence is to allow each Creator to manage and publish an application whitelist of approved applications. Once an application was added to a Creator's whitelist (presuming some offline business deal), Fans considering buying some asset via that application would be able to query the whitelist to determine if the application was approved.

Importantly, it must be possible for an application to be removed from a Creator's whitelist when appropriate - as determined by the Creator.

## Specification

A Creator's 'application whitelist' will manifest as a file on Hedera's file service. It is by creating and updating this file that a Creator will manage their application whitelist.

### Application Whitelist Schema 

The application whitelist is a JSON file listing the application's that have been approved by the Creator.

Each entry in the list stores a name and TLD URL for an application


    [
	    {
		    name: "Shopify",
		    URL: "www.shopify.com"
	    },
    ]


### Binding Whitelist to Creator

It must be possible to establish that an application whitelist (with a given Hedera File identifier) is associated with a particular Creator (as identified by their social token's token identifier). 

The social token's memo field will be used to point to the appropriate application whitelist. Rather than directly specifying the file identifier within the memo field, the memo filed will contain a Decentralized Identifier (DID) that itself 'points' to a JSON DID Document stored off chain. The DID Document will contain the File identifier of the application whitelist.

The sequence is Query Token -> Determine DID -> Resolve DID into DID Document -> Determine application whitelist file identifier -> Retrieve application whitelist JSON

While the DID model introduces an extra lookup, it provides a flexible integration point for the future. As an example, we can leverage the DID mechanism to bind a Verifiable Credential issued to the Creator to the social token in the same manner as for this application whitelist.

