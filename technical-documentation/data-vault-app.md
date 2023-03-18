---
description: Technical documentation
---

# Data Vault App

The Sismo Data Vault application is the prover from which a [zkConnect](../what-is-sismo/zkconnect.md) application requests user proofs.&#x20;

This page details the input and output specifications of the Data Vault application and the way it communicates with your application after integrating zkConnect.

<figure><img src="../.gitbook/assets/Untitled-2023-03-09-1926.png" alt=""><figcaption><p>Data Vault App flow</p></figcaption></figure>

The Data Vault application receives a `zkConnectRequest` as an input and sends a `zkConnectResponse` as an output.

The communication between your application and the Data Vault application is made only through query parameters.

Users are redirected from your application to the Data Vault application, where they generate a zero-knowledge proof. Then, they are redirected back to your application with the zero-knowledge proof in query parameters.

{% hint style="info" %}
Learn more about Sismo zero-knowledge proofs and use cases [**here**](https://docs.sismo.io/sismo-docs/technical-concepts/proof-identifier).

Learn more about zkConnect [**here**](../what-is-sismo/zkconnect.md).
{% endhint %}

## zkConnectRequest (input):

The `zkConnectRequest` is the input taken by Sismo’s Data Vault app. It defines the properties and requirements that the user must meet in order to generate a zero-knowledge proof. The Data Vault app receives the `zkConnectRequest` as a set of query parameters sent to the following base url: `https://vault-beta.sismo.io` :

* It is of type `ZkConnectRequest` , composed of the following properties: `version`(required), `appId` (required), `dataRequest` (optional), `namespace` (optional) and `callbackPath` (optional).
* The complete request url to the Data Vault app for a zero-knowledge proof is the following:\
  <mark style="color:blue;">`https://vault-beta.sismo.io/connect?version=<sismo-vault-app-version>&appId=<your-app-id>&dataRequest=<json-of-dataRequest>&namespace=<your-service-namespace>&callbackPath=<custom-callback-path>`</mark>

See the detailed `zkConnectRequest` specifications [**here**](https://www.notion.so/Data-Vault-App-V0-2-DRAFT-2d5ca38b7f5f475c911491d6acdc607d) for more details.

## zkConnectResponse (output)

The `zkConnectResponse` is the output send by the Data Vault app to the requesting application once the user has finished the zero-knowledge proof generation flow:

* It is of type `ZkConnectResponse` , composed of the following properties: `appId`, `namespace`, `verifiableStatements`, `authProof` and `version`.
* It is sent to your application as a JSON stringified object in query parameters: <mark style="color:blue;">`https://app.io/?zkConnectResponse=${JSON.stringify(zkConnectResponse)}`</mark>

See the detailed `zkConnectResponse` specifications [**here**](https://www.notion.so/Data-Vault-App-V0-2-DRAFT-2d5ca38b7f5f475c911491d6acdc607d) for more details.

## Detailed specifications

In this section, we detail the exact specifications of the `zkConnectRequest` taken as input of the Data Vault app and the `zkConnectResponse` send as output of the Data Vault app to your application, as well as the redirection url formats used to communicate between your application and the Data Vault app.

### zkConnectRequest - Redirection to the Data Vault app

The `zkConnectRequest` is the input taken by the Data Vault app.

During the proof generation flow of the Data Vault app, if there is a `dataRequest` defined in the `zkConnectRequest`, users are required to provide an eligible account that meet the criteria specified. Otherwise, there are no criteria to match and the Data Vault app will just verify that the account owns a Vault.

This section details the `zkConnectRequest` specifications and the redirection url to correctly send the `zkConnectRequest` to the Data Vault app in order for the user to generate a zero-knowledge proof associated with your zkConnect app.

#### The redirection url

A proof request url is composed of the following base url <mark style="color:blue;">`https://vault-beta.sismo.io/connect`</mark> with the `zkConnectRequest` in query parameters.

The complete proof request url with the `zkConnectRequest` in query parameters follows the following format:\
<mark style="color:blue;">`https://vault-beta.sismo.io/connect?version=<sismo-vault-app-version>&appId=<your-app-id>&dataRequest=<json-of-dataRequest>&namespace=<your-service-namespace>&callbackPath=<custom-callback-path>`</mark>

#### The `zkConnectRequest` query parameters explained:

* <mark style="color:red;">`version`</mark>_(required)_: the version of the Data Vault app queried. Currently, only the “off-chain-1” version is available.
* <mark style="color:red;">`appId`</mark> _(required)_: the unique identifier of your application registered on the Sismo Factory app.
* <mark style="color:red;">`dataRequest`</mark> _(optional)_: **a stringified JSON** of a complex object specifying the group requirements to which the user must prove that he belongs to. The current properties of the `dataRequest` object are:
  *   <mark style="color:red;">`statementRequest[]`</mark> _(required)_: an array of statements that have the following properties:

      NB: For now only 1 statement is supported in this array. But it will accept more statements in the future.

      *   <mark style="color:red;">`groupId`</mark> _(required)_: the unique identifier of the group of accounts to which the user must prove that he belongs to in order to generate the proof. Groups are maintained by the Sismo protocol and can be created by anyone using the [Sismo Factory](https://factory.sismo.io/). Groups are composed of a name, a description, specifications, a timestamp, and data. Moreover, Groups are split into Group Snapshots which are a timestamped.

          Learn more about groups [**here**](sismo-api/group/).


      * <mark style="color:red;">`groupTimeStamp`</mark> _(optional)_: by default set to “latest”. Groups are composed of snapshots generated either once, daily, or weekly. Each Group Snapshot generated has a timestamp associated to them. By default, the selected group is the latest Group Snapshot generated. But you are free to select a Group Snapshot with a different timestamp than the latest generated one.
      * <mark style="color:red;">`requestedValue`</mark> _(optional)_: by default set to “1”. A group is a mapping of account and value pairs. Each account is associated with a value. Querying a specific `value` restricts eligibility to users belonging to the group with the minimum specified `value` or matching the exact `value`
      * <mark style="color:red;">`comparator`</mark> _(optional)_: by default `GTE`. Allow choosing if we want to restrict the eligibility for the accounts that have the exact (`EQ`) or at least (`GTE`) the `requestedValue` specified before.
      * <mark style="color:red;">`provingScheme`</mark> _(optional)_: the proving scheme that the Data Vault app will use to generate the proof.
      * <mark style="color:red;">`extraData.devAddresses`</mark> _(optional)_: a mapping of accounts/values (`{account1:value1, account2:value2, ...}`). Enable to set the accounts you want to whitelist in the dev mode. This mode allows to test the full zkConnect flow without having an address eligible for a preexisting group of accounts.
  * <mark style="color:red;">`operator`</mark> _(required)_: this property is not supported for now. As long as it is not possible to set multiple statements (in the statementRequest array) this property cannot be used too.
* <mark style="color:red;">`namespace`</mark> _(optional)_: by default set to “main”. You can optionally define a `namespace` on top of the`appId` to use the zkConnect flow in different parts of your application. This is useful if you do not want users to generate the same proof for different services in your application. For example, if your application is a DAO voting website, you may define a different `namespace` for every vote and use the `proofId` as a nullifier. Henceforth, users will be able to vote only once as each proof request with a distinct `namespace` generates a distinct `proofId` for the same user. Learn more about the proof identifier [**here**](../technical-concepts/vault-and-proof-identifiers.md).
* <mark style="color:red;">`callbackPath`</mark> _(optional)_: by default, the Data Vault app redirects users to the referrer url (your application url) at the end of the proof generation flow. You can specify an optional callback path to the referrer url.

> **Example of a Data Vault app query**\
> ****\
> ****<mark style="color:blue;">`https://vault-beta.sismo.io/connect?version=off-chain-1&appId=0x112a692a2005259c25f6094161007967&dataRequest={"statementRequests":[{"groupId":"0xe9ed316946d3d98dfcd829a53ec9822e","groupTimestamp":"latest","requestedValue":1,"comparator":"GTE","provingScheme":"hydra-s1.2"}],"operator":null}&namespace=main`</mark>.
>
> ****
>
> **When we deconstruct this query, it means:**
>
> * User is requested to generate a zero-knowledge proof using the Data Vault app version: `off-chain-1`.
> * User is requested to generate a proof for the zkConnect app with an appId of `0x112a692a2005259c25f6094161007967` and for the namespace `main`.
> * User is requested to prove that he belongs to the `latest` group snapshot of the identified group `0xe9ed316946d3d98dfcd829a53ec9822e` (here the `sismo-contributors` group).
> * User is requested to prove that he has a minimum value registered in the group snapshot of `1`.
>
>
>
> **HTML integration example:**
>
> ```html
> //Example of proof request integration without using the PwS client package
> <html>
>   <body>
>     <a
>       href='<https://vault-beta.sismo.io/connect?version=off-chain-1&appId=0x112a692a2005259c25f6094161007967&dataRequest={"statementRequests":[{"groupId":"0xe9ed316946d3d98dfcd829a53ec9822e","groupTimestamp":"latest","requestedValue":1,"comparator":"GTE","provingScheme":"hydra-s1.2"}>],"operator":null}&namespace=main'
>       >Prove with Sismo</a
>     >
>   </body>
> </html>
> ```

### zkConnectResponse - Output of the Data Vault app

The `zkConnectResponse` is the output of the Data Vault app.

Once the user has imported an eligible account into the Data Vault app that meets the criteria of your `zkConnectRequest` ([see above](https://www.notion.so/Data-Vault-App-V0-2-DRAFT-2d5ca38b7f5f475c911491d6acdc607d)) the app generates a `zkConnectResponse` and sends it back to your application.

This section details the `zkConnectResponse` specifications and how it is sent from the Data Vault app to your application.

#### Redirection url to your application

At the end of the proving scheme, the Data Vault app redirects to the referrer url (your application url), appending the `zkConnectResponse` in a JSON stringified format as a query parameter: <mark style="color:blue;">`https://www.app.io?zkConnectResponse=${JSON.stringify(zkConnectResponse)}`</mark>

#### The `zkConnectResponse` explained

The `zkConnectResponse` is composed of the following properties:

* <mark style="color:red;">`version`</mark>: the version of the Data Vault app that was used to generate the proof. Currently, only the “off-chain-1” version is available.
* <mark style="color:red;">`appId`</mark>: the unique identifier of your application registered on the Sismo Factory app.
* <mark style="color:red;">`namespace`</mark>: the name of your service defined during the proof request. If no service name was provided during the proof request, the service name by default `main` is returned.
* <mark style="color:red;">`verifiableStatements[]`</mark>: an array of `verifiableStatement` composed of the following properties:
  * <mark style="color:red;">`groupId`</mark>: the group identifier used to check if the user was eligible in order to generate the zero-knowledge proof.
  * <mark style="color:red;">`groupTimeStamp`</mark>: the timestamp of the group snapshot for which the user had to be eligible to in order to generate the zero-knowledge proof.
  * <mark style="color:red;">`requestedValue`</mark>: the value against which the user had to be eligible to in order to generate the zero-knowledge proof.
  * <mark style="color:red;">`comparator`</mark>: the value comparator used to restrict the eligibility for the accounts that have the exact (`EQ`) `requestedValue` specified before or have an equal or greater value (`GTE`).
  * <mark style="color:red;">`provingScheme`</mark>: the proving scheme name used during the proof generation flow. Currently (and by default), only the “hydra-s2.1” proving scheme is available.
  * <mark style="color:red;">`extraData.devAddresses`</mark>: the list of whitelisted accounts for the dev mode defined in the `zkConnectResponse`. It allows developers to add their addresses and compute cryptographically valid proofs when redirected to the Sismo Developer Vault.
  * <mark style="color:red;">`value`</mark>: the value against which the user was eligible to in order to generate the zero-knowledge proof.
  * <mark style="color:red;">`proof`</mark>: the actual cryptographic proof returned by the proving scheme.
* <mark style="color:red;">`authProof`</mark>: it is used only when there is no `dataRequest` in the `zkConnectRequest`
  * <mark style="color:red;">`provingScheme`</mark>: the proving scheme name used during the proof generation flow. Currently (and by default), only the “hydra-s2.1” proving scheme is available.
  * <mark style="color:red;">`proof`</mark>: the cryptographic proof returned by the proving scheme when there is no `dataRequest` defined. So it’s basically a proof of vault ownership.

#### How to read the `zkConnectResponse` on your application

```tsx
// in your application
<html>
  <script>
      const urlParams = new URLSearchParams(window.location.search);
      const pwsProof = urlParams.get("zkConnectResponse");
      console.log(JSON.parse(pwsProof));
  </script>
</html>
```

#### Verifying the `zkConnectResponse` validity

The `zkConnectResponse` validity must be verified by your application backend.

To do checkout the [zkConnect packages docs ](zkconnect/)and follow the [zkConnect package tutorial](../tutorials/zkconnect/zk-connect-guide.md).
