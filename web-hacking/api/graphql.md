# GraphQL

{% hint style="info" %}
GraphQL is a query language and server-side runtime for application programming interfaces (APIs) that prioritizes giving clients exactly the data they request and no more.&#x20;

GraphQL is designed to make APIs fast, flexible, and developer-friendly. It can even be deployed within an integrated development environment (IDE) known as GraphiQL. As an alternative to REST, GraphQL lets developers construct requests that pull data from multiple data sources in a single API call.&#x20;
{% endhint %}

## Tools

{% embed url="https://github.com/Escape-Technologies/graphinder" %}

### graphinder

<pre class="language-bash"><code class="lang-bash"><strong>pip install graphinder
</strong># using specific python binary
python3 -m pip install graphinder

graphinder -d $domain
</code></pre>

### graphqlmap

```bash
# installation
git clone https://github.com/swisskyrepo/GraphQLmap.git
cd GraphQLmap/
python setup.py install
# usage
graphqlmap -h
```

## Introspection enabled

```json
{__schema{queryType{name}mutationType{name}subscriptionType{name}types{...FullType}directives{name description locations args{...InputValue}}}}fragment FullType on __Type{kind name description fields(includeDeprecated:true){name description args{...InputValue}type{...TypeRef}isDeprecated deprecationReason}inputFields{...InputValue}interfaces{...TypeRef}enumValues(includeDeprecated:true){name description isDeprecated deprecationReason}possibleTypes{...TypeRef}}fragment InputValue on __InputValue{name description type{...TypeRef}defaultValue}fragment TypeRef on __Type{kind name ofType{kind name ofType{kind name ofType{kind name ofType{kind name ofType{kind name ofType{kind name ofType{kind name}}}}}}}}
```

{% embed url="https://ivangoncharov.github.io/graphql-voyager/" %}
Copy the output and put it into this
{% endembed %}

## Introspection not enabled

### Fuzz

{% embed url="https://github.com/nikitastupin/clairvoyance" %}

{% embed url="https://github.com/swisskyrepo/GraphQLmap" %}

## Dump DB schema

```bash
# url encoded
fragment+FullType+on+__Type+{++kind++name++description++fields(includeDeprecated%3a+true)+{++++name++++description++++args+{++++++...InputValue++++}++++type+{++++++...TypeRef++++}++++isDeprecated++++deprecationReason++}++inputFields+{++++...InputValue++}++interfaces+{++++...TypeRef++}++enumValues(includeDeprecated%3a+true)+{++++name++++description++++isDeprecated++++deprecationReason++}++possibleTypes+{++++...TypeRef++}}fragment+InputValue+on+__InputValue+{++name++description++type+{++++...TypeRef++}++defaultValue}fragment+TypeRef+on+__Type+{++kind++name++ofType+{++++kind++++name++++ofType+{++++++kind++++++name++++++ofType+{++++++++kind++++++++name++++++++ofType+{++++++++++kind++++++++++name++++++++++ofType+{++++++++++++kind++++++++++++name++++++++++++ofType+{++++++++++++++kind++++++++++++++name++++++++++++++ofType+{++++++++++++++++kind++++++++++++++++name++++++++++++++}++++++++++++}++++++++++}++++++++}++++++}++++}++}}query+IntrospectionQuery+{++__schema+{++++queryType+{++++++name++++}++++mutationType+{++++++name++++}++++types+{++++++...FullType++++}++++directives+{++++++name++++++description++++++locations++++++args+{++++++++...InputValue++++++}++++}++}}

# full
fragment FullType on __Type {
  kind
  name
  description
  fields(includeDeprecated: true) {
    name
    description
    args {
      ...InputValue
    }
    type {
      ...TypeRef
    }
    isDeprecated
    deprecationReason
  }
  inputFields {
    ...InputValue
  }
  interfaces {
    ...TypeRef
  }
  enumValues(includeDeprecated: true) {
    name
    description
    isDeprecated
    deprecationReason
  }
  possibleTypes {
    ...TypeRef
  }
}
fragment InputValue on __InputValue {
  name
  description
  type {
    ...TypeRef
  }
  defaultValue
}
fragment TypeRef on __Type {
  kind
  name
  ofType {
    kind
    name
    ofType {
      kind
      name
      ofType {
        kind
        name
        ofType {
          kind
          name
          ofType {
            kind
            name
            ofType {
              kind
              name
              ofType {
                kind
                name
              }
            }
          }
        }
      }
    }
  }
}

query IntrospectionQuery {
  __schema {
    queryType {
      name
    }
    mutationType {
      name
    }
    types {
      ...FullType
    }
    directives {
      name
      description
      locations
      args {
        ...InputValue
      }
    }
  }
}
```

## List path

```bash
git clone https://gitlab.com/dee-see/graphql-path-enum.git
cd graphql-path-enum/
graphql-path-enum --help
```

## Vulnerabilities

* **SQL Injection**: simple but classic, try SQL and NoSQL injection in fields values,
  * Send a single quote ' inside a graphql parameter to trigger the SQL injection
* **Debug & information disclosure**: Insert bad characters in object or fields name, sometimes DEBUG mode is activated and even if you have a 403 status, you could have a good surprise,
* **Batching Attack**: Batching is the process of taking a group of requests, combining them into one, and making a single request with the same data that all of the other queries would have made (more here). When authentication process is used with GraphQL, batch attack can be performed to simultaneously sending many queries with different credentials, it’s like a bruteforce attack but only with one request. Also, batch attack can be used against 2FA authentication, to bypass rate-limit (if it’s based on number of query by IP for example). More : https://lab.wallarm.com/graphql-batching-attack/
