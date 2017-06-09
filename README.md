# ADFS Metadata Parser
[![NuGet Status](https://img.shields.io/nuget/v/DrWatson.Adfs.Metadata.svg?style=flat)](https://www.nuget.org/packages/DrWatson.Adfs.Metadata)

It is a small C# library to load and parse ADFS metadata. Parser was tested on ADFS 3.0.

## Features

* Asynchronous loading a metadata directly from ADFS 3.0 server
* Caching metadata between calls
* Ability to reload if needed
* DI-Container friendly
* Supported metadata (it will be expanded later)
    * Federation server identity
    * Signing certificate
    * More features coming soon... ))

## Installing

```bash
Install-Package DrWatson.Adfs.Metadata -pre
```

## Usage

```csharp
AdfsMetadataService svc = new AdfsMetadataLoader(() =>
{
    return new HttpClient().GetStringAsync(
        "https://fs.example.com/FederationMetadata/2007-06/FederationMetadata.xml"
    );
});
// Exception can be thrown
var metadata = await svc.Get();

// Subsequent  calls will return result from cache
metadata = await svc.Get();

// Now we can use metadata as:
string IdP = metadata.Identity;
string stringSigningCert = metadata.SigningCertificateString;

// Or get certificate with the extension method:
X509Certificate2 signingCert = metadata.GetSigningCertificate();

// Start reloading
svc.Invalidate();

// Now we have a new metadata
metadata = await svc.Get();
```
