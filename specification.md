# Collections Manifest

The collection manifest describes the assets and remote repositories which are part of a representative collection. A collection is versioned. 

For an example see [experimental/java-microprofile-0.2.1/collection.yaml](experimental/java-microprofile-0.2.1/collection.yaml)

### Manifest Contents

A collection manifest contains a series of collection specific attributes followed by a list of asset references. 

Each asset reference may include one or more of the following: 
* Asset Name
* Asset Digest Checksum (SHA-256 format)
* Asset Type

### Supported Asset Types

Asset type may be: 

* appsody - indicates that the referenced asset is an appsody stack
* kubernetes-resource - the referenced asset is a kubernetes resource which will be 'kubectl applied' when the collection is instantiated on a Kubernetes cluster

### Appsody

A collection may reference at most one Appsody Stack. When the type is 'appsody', the URL reference must refer to the bundled archive format of the appsody stack. An example of such as URL is: [https://github.com/appsody/stacks/releases/download/0.0.1-alpha/incubator.java-microprofile.templates.default.tar.gz](https://github.com/appsody/stacks/releases/download/0.0.1-alpha/incubator.java-microprofile.templates.default.tar.gz)

# Collection Archives

A collection manifest and a copy of any of the required assets can be assembled into a collection archive. 

*Referenced repositories*: Airgapping and importing a collection would not guarantee it's in a workable state because the remote referenced repositories might not be available
*Transmission of Deltas*: Are deltas represented? Can you import just the changes or do you always re-import an entire collection? 

# Repository

collections may be served from a collection Repository. A collection repository includes an index which references all the available collections. See [examples/index.yaml](examples/index.yaml)

A collection repository may include: 
* A git repository
* An http file server (such as object storage)
* A docker registry

## Index

A collection index is a superset of the Appsody index. This allows the Appsody toolset to navigate the Kabanero Repository as though it were an Appsody repository. 

### Collection Referents

The collection URL referred to by the index can be one of: 

* A collection archive in tar.gz format
* A remote directory containing a collection.yaml file. The collection.yaml URL must be found when appending either /collection.yaml to the provided URL or collection.yaml to the provided URL
* A remote collection.yaml file

When attempting to navigate into a collection from the index, a client must analyze the URL and determine whether the remote URL represents the expanded/source form of the collection or a collection archive. The client makes this determination by analyzing the URL file extension. If it ends with 'tar.gz', the collection is determined to be in the archive form, otherwise the collection is in expanded/source form. In it's expanded source form, the `collections.yaml` file is to be found by appending collections.yaml to the provided URL. In binary form, the `collections.yaml` file must be located in the root of the archive. 

Examples:

* http://myurl/ - http://myurl/collections.yaml is the collection manifest
* http://myurl - http://myurl/collections.yaml is the collection manifest
* http://myurl/collection.tar.gz - collections.yaml is found within the provided archive
