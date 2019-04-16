---

copyright:
  years: 2018-2019
lastupdated: "2019-04-01"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}

# CIS CLI Commands

## CIS commands index
{: #commands_index}

The IBM® Cloud Internet Services has the following families of commands available from the command line interface (CLI).

Find the full set of commands for Cloud Internet Services (CIS) within each set, including `Create`, `Delete`, `Update`, and so forth.
{: shortdesc}

## Overview
{: #overview}

View the overview information for an instance.

**NAME**

  `cloud-internet-services overview` - Show the overview information for an instance. 

**USAGE**

  `ibmcloud cis overview DNS_DOMAIN_ID [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**ARGUMENTS**

   * `DNS_DOMAIN_ID`  The ID of DNS domain.
   
**OPTIONS**

   * `-i, --instance INSTANCE_NAME`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.
   * `--output FORMAT`    Specify output format, only JSON is supported now.
              
   * `Service Mode` is displayed only when the domain is paused or in defense mode.

## Cache
{: #cache}

Manipulate how the cache performs using the following `cache` commands:
### Purge Cache
{: #purge-cache}

**NAME**

  `cache-purge` -  Clear cached assets file by file or entirely for a given DNS domain to guarantee served assets are updated.

**USAGE**

  `ibmcloud cis cache-purge DNS_DOMAIN_ID (--all | -f, --file file1,file2,...) [-i, --instance INSTANCE_NAME]`

**ARGUMENTS**

   * `DNS_DOMAIN_ID`  The ID of DNS domain.

**OPTIONS**

   * `-i, --instance INSTANCE_NAME` (Optional) Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.

   *  `--all` Clear cached assets entirely, when this option exists, users are asked to confirm `Y/N`. Purging all cached files increases page load times while the files are being re-created. This process removes all of your cached data on all edge servers.

   * `-f, --file file1,file2,...`  an array of files (separated by commas) that should be cleared from cache. You may purge up to 30 files at a time with comma-separated files. To specify a file, you must enter the full path. Wildcards are not supported at this time.


### Show cache settings
{: #show-cache}

**NAME**

  `cache-settings` - Get caching settings for a given DNS domain.

**USAGE**

  `ibmcloud cis cache-settings DNS_DOMAIN_ID [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**ARGUMENTS**

   * `DNS_DOMAIN_ID` The ID of DNS domain.

**OPTIONS**

   * `-i, --instance INSTANCE_NAME` Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.
  
   * `--output FORMAT`   Specify output format, only JSON is supported now.
   
### Update cache settings
{: #update-cache}

**NAME**

  `cache-settings-update` - Update cache settings for a give DNS domain.

**USAGE**

  `ibmcloud cis cache-settings-update DNS_DOMAIN_ID [--caching-level LEVEL][--browser-expiration EXPIRATION] [--development-mode (on | off)] [--serve-stale-content (on | off)] [--query-string-sort (on | off)] [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**ARGUMENTS**

   * `DNS_DOMAIN_ID`  The ID of DNS domain.

**OPTIONS**

   * `--caching-level _value_`  Specify under what URL conditions you want to deliver cached assets to the user.  Valid _value_ tokens are: `no-query-string`,  `query-string-independent`,  `query-string-dependent`.
   
     * `no-query-string` Only delivers resources from cache when there is no query string.                    
     * `query-string-independent` Delivers the same resource to everyone independent of the query string.
     * `query-string-dependent` Delivers a different resource each time the query string changes.


   * `--browser-expiration _value_` Specify how long you want the user's browser to store cached assets.                Valid _value_ tokens are: `respect-existing-header`, `30M`, `1h`, `2h`, `4h`, `8h`, `16h`, `1d`, `3d`, `8d`, `16d`, `1m`, `6m`, `1y`.                         
     * `30M` means `30 minutes`
     * `1h` means `1 hour`
     * `1d` means `1 day`
     * `1m` means `1 month`
     * `1y` means `1 year`


   * `--development-mode _value_` Bypass all edge caches and send traffic toward your origin servers.


   * `--serve-stale-content _value_`  Continue serving cached content to users when origin servers are offline, even if the content has expired.

   * `--query-string-sort _value_`    In the cache, CIS treats files with the same query strings as the same file, regardless of the order of the query strings.

   * `-i, --instance INSTANCE_NAME`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.

   * `--output FORMAT`   Specify output format, only JSON is supported now.

## DNS Record
{: #dns-record}

Manipulate how the DNS Record performs using the following `dns-record` commands:
### Create DNS record
{: #create-dns-record}

**NAME**

   `dns-record-create` - Create a DNS record for a given domain of a service instance.

**USAGE**

   `ibmcloud cis dns-record-create DNS_DOMAIN_ID (-s, --json-str JSON_STR | -j, --json-file JSON_FILE) [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**ARGUMENTS**

   * `DNS_DOMAIN_ID` is the ID of DNS domain.

**OPTIONS**

   `-s, --json-str`  The JSON data used to describe a DNS Record.

   Supported DNS Record types are: `A`, `AAAA`, `CNAME`, `NS`, `TXT`, `SPF`, `MX`, `LOC`, `SRV`, `CAA`.

  * For type `A`, `AAAA`, `CNAME`, `NS`, `TXT`, `SPF`:
    * The required fields in JSON data are `name`, `type`, `content`:
    * The optional fields are `ttl`, `proxied`:
      * `proxied` Control whether traffic should flow through the security and performance functions on CIS. Can be used for update only.

   Sample JSON data:


                   {
                       "name": "testA",
                       "type": "A",
                       "content": "127.0.0.1"
                   }

                   {
                       "name": "testAAAA",
                       "type": "AAAA",
                       "content": "2001:0db8:0012:0001:3c5e:7354:0000:5db1"
                   }

                   {
                       "name": "testCNAME",
                       "type": "CNAME",
                       "content": "example.com"
                   }

                   {
                       "name": "testNS",
                       "type": "NS",
                       "content": "ns1.example.com"
                   }

                   {
                       "name": "testTXT",
                       "type":"TXT",
                       "content": "text information"
                   }

                   {
                       "name": "testSPF",
                       "type":"SPF",
                       "content": "v=spf1 a ip:1.2.3.4"
                   }


   * For type `MX`:
     * The required fields in JSON data are `name`, `type`, `content`:
     * The optional fields are `ttl`, `priority`, `proxied`:
       * `proxied` Controls whether traffic should flow through the security and performance functions on CIS. Can be used for update only.

   Sample JSON data:

                   {
                       "name": "testMX",
                       "type": "MX",
                       "content": "smtp.example.com",
                       "priority": 10
                   }


   * For type `LOC`:
     * The required fields in JSON data are `name`, `type`, `data`:

                       "data":
                           "lat_degrees": Degrees of latitude.
                           "lat_minutes": Minutes of latitude
                           "lat_seconds": Seconds of latitude.
                           "lat_direction": Latitude direction.
                           "long_degrees": Degrees of longitude.
                           "long_minutes": Minutes of longitude.
                           "long_seconds": Seconds of longitude.
                           "long_direction": Longitude direction.
                           "altitude": Altitude of location in meters.
                           "size": Size of location in meters.
                           "precision_horz": Horizontal precision of location.
                           "precision_vert": Vertical precision of location.

     * The optional fields are `ttl`, `proxied`:
       * `proxied` Controls whether traffic should flow through the security and performance functions on CIS. Can be used for update only.

   Sample JSON data:

                   {
                       "name": "testLOC",
                       "type": "LOC",
                       "data": {
                            "lat_degrees": 45,
                            "lat_minutes": 0,
                            "lat_seconds": 0,
                            "lat_direction": "N",
                            "long_degrees": 45,
                            "long_minutes": 0,
                            "long_seconds": 0,
                            "long_direction": "E",
                            "altitude": 20,
                            "size": 0,
                            "precision_horz": 0,
                            "precision_vert": 0
                       }
                   }


   * For type `SRV`:
     * The required fields in JSON data are `type`, `data`:

                       "data":
                           "service": A service type, prefixed with an underscore.
                           "proto": A valid protocol.
                           "priority": Priority.
                           "weight": The record weight.
                           "port": The port of the service.
                           "target": A valid hostname.

     * The optional fields are `ttl`, `proxied`:
       * `proxied` Controls whether traffic should flow through the security and performance functions on CIS. Can be used for update only.

   Sample JSON data:

                   {
                       "type": "SRV",
                       "data": {
                            "service": "_ftp",
                            "proto": "_tcp",
                            "name": "testSRV",
                            "priority": 1,
                            "weight": 1,
                            "port": 21,
                            "target": "example.com"
                       }
                   }

   * For type `CAA`:
     * The required fields in JSON data are name, `type`, `data`:
     * The optional fields are `ttl`, `proxied`:
       * `proxied` Controls whether traffic should flow through the security and performance functions on CIS. Can be used for update only.

   Sample JSON data:

                   {
                       "name": "testCAA.yourdomain.com",
                       "type": "CAA",
                       "data": {
                            "tag": "issue",
                            "value": "letsencrypt.org"
                       }
                   }
   `-j, --json-file`  A file contains input JSON data.

   `-i, --instance INSTANCE_NAME`   Instance name. If the name is not set, the context instance specified by `ibmcloud cis instance-set` is used.

   `--output FORMAT`     Specify output format, only JSON is supported now.


### Update DNS record
{: #update-dns-record}

**NAME**

   `dns-record-update` - Update a DNS record for a given domain of a service instance.

**USAGE**

   `ibmcloud cis dns-record-update DNS_DOMAIN_ID DNS_RECORD_ID (-s, --json-str JSON_STR | -j, --json-file JSON_FILE) [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**ARGUMENTS**

   `DNS_DOMAIN_ID` is the ID of DNS domain.

   `DNS_RECORD_ID` is the ID of DNS record.

**OPTIONS**

   `-s, --json-str`  The JSON data used to describe a DNS Record.

   Supported DNS Record types are: `A`, `AAAA`, `CNAME`, `NS`, `TXT`, `SPF`, `MX`, `LOC`, `SRV`, `CAA`.

   * For type `A`, `AAAA`, `CNAME`, `NS`, `TXT`, `SPF`:
     * The required fields in JSON data are `name`, `type`, `content`:
     * The optional fields are `ttl`, `proxied`:
       * `proxied` Controls whether traffic should flow through the security and performance functions on CIS. Can be used for update only.

  Sample JSON data:


                   {
                       "name": "testA",
                       "type": "A",
                       "content": "127.0.0.1"
                   }

                   {
                       "name": "testAAAA",
                       "type": "AAAA",
                       "content": "2001:0db8:0012:0001:3c5e:7354:0000:5db1"
                   }

                   {
                       "name": "testCNAME",
                       "type": "CNAME",
                       "content": "example.com"
                   }

                   {
                       "name": "testNS",
                       "type": "NS",
                       "content": "ns1.example.com"
                   }

                   {
                       "name": "testTXT",
                       "type":"TXT",
                       "content": "text information"
                   }

                   {
                       "name": "testSPF",
                       "type":"SPF",
                       "content": "v=spf1 a ip:1.2.3.4"
                   }




   * For type `MX`:
     * The required fields in JSON data are `name`, `type`, `content`:
     * The optional fields are `ttl`, `priority`, `proxied`:
       * `proxied` Controls whether traffic should flow through the security and performance functions on CIS. Can be used for update only.

   Sample JSON data:

                   {
                       "name": "testMX",
                       "type": "MX",
                       "content": "smtp.example.com",
                       "priority": 10
                   }




   * For type `LOC`:
     * The required fields in JSON data are `name`, `type`, `data`:

                       "data":
                           "lat_degrees": Degrees of latitude.
                           "lat_minutes": Minutes of latitude
                           "lat_seconds": Seconds of latitude.
                           "lat_direction": Latitude direction.
                           "long_degrees": Degrees of longitude.
                           "long_minutes": Minutes of longitude.
                           "long_seconds": Seconds of longitude.
                           "long_direction": Longitude direction.
                           "altitude": Altitude of location in meters.
                           "size": Size of location in meters.
                           "precision_horz": Horizontal precision of location.
                           "precision_vert": Vertical precision of location.

     * The optional fields are `ttl`, `proxied`:
       * `proxied` Controls whether traffic should flow through the security and performance functions on CIS. Can be used for update only.

   Sample JSON data:

                   {
                       "name": "testLOC",
                       "type": "LOC",
                       "data": {
                            "lat_degrees": 45,
                            "lat_minutes": 0,
                            "lat_seconds": 0,
                            "lat_direction": "N",
                            "long_degrees": 45,
                            "long_minutes": 0,
                            "long_seconds": 0,
                            "long_direction": "E",
                            "altitude": 20,
                            "size": 0,
                            "precision_horz": 0,
                            "precision_vert": 0
                       }
                   }




  * For type `SRV`:
     * The required fields in JSON data are `type`, `data`:

                       "data":
                           "service": A service type, prefixed with an underscore.
                           "proto": A valid protocol.
                           "priority": Priority.
                           "weight": The record weight.
                           "port": The port of the service.
                           "target": A valid hostname.

     * The optional fields are `ttl`, `proxied`:
       * `proxied` Controls whether traffic should flow through the security and performance functions on CIS. Can be used for update only.

   Sample JSON data:

                   {
                       "type": "SRV",
                       "data": {
                            "service": "_ftp",
                            "proto": "_tcp",
                            "name": "testSRV",
                            "priority": 1,
                            "weight": 1,
                            "port": 21,
                            "target": "example.com"
                       }
                   }



   * For type `CAA`:
     * The required fields in JSON data are `name`, `type`, `data`:
     * The optional fields are `ttl`, `proxied`:
       * `proxied` Controls whether traffic should flow through the security and performance functions on CIS. Can be used for update only.

   Sample JSON data:

                   {
                       "name": "testCAA.yourdomain.com",
                       "type": "CAA",
                       "data": {
                            "tag": "issue",
                            "value": "letsencrypt.org"
                       }
                   }
                   
    `-j, --json-file`   A file contains input JSON data.

   `-i, --instance INSTANCE_NAME`   Instance name. If the name is not set, the context instance specified by `ibmcloud cis instance-set` is used.

   `--output FORMAT`     Specify output format, only JSON is supported now.


### Get DNS record
{: #get-dns-record}

**NAME**

   `dns-record` - Get a DNS record details for a given domain under a service instance.

**USAGE**

   `ibmcloud cis dns-record DNS_DOMAIN_ID DNS_RECORD_ID [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**ARGUMENTS**

   `DNS_DOMAIN_ID` is the ID of DNS domain.

   `DNS_RECORD_ID` is the ID of DNS record.

**OPTIONS**

   `-i, --instance INSTANCE_NAME`  Instance name. If the name is not set, the context instance specified by `ibmcloud cis instance-set` is used.

   `--output FORMAT`    Specify output format, only JSON is supported now.


### Delete DNS record
{: #delete-dns-record}

**NAME**

   `dns-record-delete` - Delete a DNS record for a given domain of a service instance.

**USAGE**

   `ibmcloud cis dns-record-delete DNS_DOMAIN_ID DNS_RECORD_ID [-i, --instance INSTANCE_NAME]`

**ARGUMENTS**

   `DNS_DOMAIN_ID` is the ID of DNS domain.

   `DNS_RECORD_ID` is the ID of DNS record.

**OPTIONS**

   `-i, --instance INSTANCE_NAME`  Instance name. If the name is not set, the context instance specified by `ibmcloud cis instance-set` is used.
   
   

### List DNS Records
{: #list-dns-record}

**NAME**

   `dns-records` - List all DNS records for a given domain of a service instance.

**USAGE**

   `ibmcloud cis dns-records DNS_DOMAIN_ID (-s, --json-str JSON_STR | -j, --json-file JSON_FILE) [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**ARGUMENTS**

   `DNS_DOMAIN_ID` is the ID of DNS domain.

**OPTIONS**

   `-s, --json-str`  The JSON data to query DNS records.

The optional fields are `type`, `name`, `content`, `page`, `per_page`, `order`, `direction`,`match`:

  * `type` Type of DNS records to display.      
  * `name` Value of name field to filter by.
  * `content` Value of content field to filter by.
  * `page` Page number of paginated results.
  * `per_page` Maximum number of DNS records per page.
  * `order` Field by which to order list of DNS records. Valid values are `type`, `name`, `content`, `ttl`, `proxied`
  * `direction` Direction in which to order results [ascending or descending order]. Valid values are `asc`, `desc`         
  * `match` Whether to match all or at least one search parameter. Valid values are `any`, `all`.

  Sample JSON data:

                   {
                       "type": "CAA",
                       "page": 1,
                       "per_page": 20
                   }
                   
   `-j, --json-file`  A file contains input JSON data.

   `-i, --instance INSTANCE_NAME`   Instance name. If name is not set, the context instance specified by `ibmcloud cis instance-set` is used.

   `--output FORMAT`     Specify output format, only JSON is supported now.


**Output Table Columns**
   * ID
   * Name
   * Type
   * Content
   * Proxied
   * TTL

### Upload your BIND config

**NAME**

   `dns-records-import` - Upload your BIND config.

**USAGE**

   `ibmcloud cis dns-records-import DNS_DOMAIN_ID (--file FILE) [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**ARGUMENTS**

   `DNS_DOMAIN_ID` is the ID of DNS domain.

**OPTIONS**

  `--file`                BIND config to upload.

   `-i, --instance INSTANCE_NAME`   Instance name. If not set, the context instance specified by 'ibmcloud cis instance-set' is used.

   `--output FORMAT`    Specify output format, only JSON is supported now.


### Export BIND config

**NAME**

   `dns-records-export` - Export BIND config.

**USAGE**

   `ibmcloud cis dns-records-export DNS_DOMAIN_ID [--file FILE] [-i, --instance INSTANCE_NAME]`

**ARGUMENTS**

   `DNS_DOMAIN_ID` is the ID of DNS domain.

**OPTIONS**

  `--file`                The BIND config saves exported DNS records..

   `-i, --instance INSTANCE_NAME`   Instance name. If not set, the context instance specified by 'ibmcloud cis instance-set' is used.

## Domain settings
{: #domain-settings}

Manipulate domain settings using the following `domain-settings` commands:

### Display domain-settings
{: #display-domain-settings}

**NAME**

   `domain-settings` - Get details of a feature for the domain.

**USAGE**

   `ibmcloud cis domain-settings DNS_DOMAIN_ID (-g, --group GROUP | -f, --feature FEATURE) [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**ARGUMENTS**

   `DNS_DOMAIN_ID` is the id of DNS domain.

**OPTIONS**

   `-g, --group value`    Display features in a same group.Valid values for "group" are "all", "domain", "reliability", "performance", "security".

   `-f, --feature value`  Feature of domain settings to check. Valid values are as follow: 
   
                               "always_use_https": 
                                   Redirect all requests with scheme "http" to "https". This applies to all http requests to the domain.
                               "automatic_https_rewrites": 
                                   Help fix mixed content by changing "http" to "https" for all resources or links on your web site that can be served with HTTPS.
                               "browser_check": 
                                   Evaluate HTTP headers from your visitors browser for threats. If a threat is found a block page will be delivered.
                               "challenge_ttl": 
                                   Specify how long a visitor with a bad IP reputation is allowed access to your website after completing a challenge.
                               "cname_flattening": 
                                   Follow a CNAME to where it points and return that IP address instead of the CNAME record.
                                   By default, only flatten the CNAME at the root of your domain.
                               "hotlink_protection": 
                                   Protect your images from off-site linking.
                               "http2": 
                                   Accelerate your website with HTTP/2.
                               "image_load_optimization": 
                                   Improve load time for pages that include images on mobile devices with slow network connections. (enterprise plan only)
                               "image_size_optimization": 
                                   Improve image load time by optimizing images hosted on your domain. (enterprise plan only)
                               "ip_geolocation": 
                                   Include the country code of the visitor location with all requests to your website.
                               "ipv6": 
                                   Enable IPv6 support and gateway.
                               "max_upload": 
                                   The amount of data visitors can upload to your website in a single request.
                               "min_tls_version": 
                                   Only allow HTTPS connections from visitors that support the selected TLS protocol version or newer.
                               "minify": 
                                   Reduce the file size of source code on your website.
                               "mobile_redirect": 
                                   Redirect visitors that are using mobile devices to a mobile-optimized website.
                               "opportunistic_encryption": 
                                   Opportunistic Encryption allows browsers to benefit from the improved performance of HTTP/2 
                                   by letting them know that your site is available over an encrypted connection.
                               "origin_error_page_pass_thru": 
                                   When Origin Error Page is set to "On", CIS will proxy the 502 and 504 error pages directly from the origin. (enterprise plan only)
                               "prefetch_preload": 
                                   CIS will prefetch any URLs included in the prefetch HTTP header. (enterprise plan only)
                               "pseudo_ipv4": 
                                   Adds an IPv4 header to requests when a client is using IPv6, but the server only supports IPv4.
                               "response_buffering": 
                                   Enable or disable buffering of responses from the origin server. (enterprise plan only)
                               "script_load_optimization": 
                                   Improve the paint time for pages that include JavaScript. (enterprise plan only)
                               "security_header": 
                                   Enforce web security policy for your website.
                               "server_side_exclude": 
                                   Automatically hide specific content from suspicious visitors.
                               "sha1_support": 
                                   Enable support for legacy user agents that do not support certificates signed with modern SHA-2 signatures.
                               "tls_client_auth": 
                                   TLS client certificate presented for authentication on origin pull. (enterprise plan only)
                               "true_client_ip_header": 
                                   CIS will send the end user’s IP address in the True-Client-IP header. (enterprise plan only)
                               "waf": 
                                   A Web Application Firewall (WAF) blocks requests that contain malicious content.
                               "websockets": 
                                   Allow WebSockets connections to your origin server.
                                   
   `-i, --instance INSTANCE_NAME`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.

   `--output FORMAT`    Specify output format, only JSON is supported now.

### Update domain settings
{: #update-domain-settings}

**NAME**

   `domain-settings-update` - Update a feature for the domain.

**USAGE**

   `ibmcloud cis domain-settings-update DNS_DOMAIN_ID (-f, --feature FEATURE) (-v, --value VALUE) [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**ARGUMENTS**

   `DNS_DOMAIN_ID` is the id of DNS domain.

**OPTIONS**

   `-f, --feature value`      The Domain feature to get Valid values are as follow: 
   
                               "always_use_https": 
                                   Redirect all requests with scheme "http" to "https". This applies to all http requests to the domain.
                               "automatic_https_rewrites": 
                                   Help fix mixed content by changing "http" to "https" for all resources or links on your web site that can be served with HTTPS.
                               "browser_check": 
                                   Evaluate HTTP headers from your visitors browser for threats. If a threat is found a block page will be delivered.
                               "challenge_ttl": 
                                   Specify how long a visitor with a bad IP reputation is allowed access to your website after completing a challenge.
                               "cname_flattening": 
                                   Follow a CNAME to where it points and return that IP address instead of the CNAME record.
                                   By default, only flatten the CNAME at the root of your domain.
                               "hotlink_protection": 
                                   Protect your images from off-site linking.
                               "http2": 
                                   Accelerate your website with HTTP/2.
                               "image_load_optimization": 
                                   Improve load time for pages that include images on mobile devices with slow network connections. (enterprise plan only)
                               "image_size_optimization": 
                                   Improve image load time by optimizing images hosted on your domain. (enterprise plan only)
                               "ip_geolocation": 
                                   Include the country code of the visitor location with all requests to your website.
                               "ipv6": 
                                   Enable IPv6 support and gateway.
                               "max_upload": 
                                   The amount of data visitors can upload to your website in a single request.
                               "min_tls_version": 
                                   Only allow HTTPS connections from visitors that support the selected TLS protocol version or newer.
                               "minify": 
                                   Reduce the file size of source code on your website.
                               "mobile_redirect": 
                                   Redirect visitors that are using mobile devices to a mobile-optimized website.
                               "opportunistic_encryption": 
                                   Opportunistic Encryption allows browsers to benefit from the improved performance of HTTP/2 
                                   by letting them know that your site is available over an encrypted connection.
                               "origin_error_page_pass_thru": 
                                   When Origin Error Page is set to "On", CIS will proxy the 502 and 504 error pages directly from the origin. (enterprise plan only)
                               "prefetch_preload": 
                                   CIS will prefetch any URLs included in the prefetch HTTP header. (enterprise plan only)
                               "pseudo_ipv4": 
                                   Adds an IPv4 header to requests when a client is using IPv6, but the server only supports IPv4.
                               "response_buffering": 
                                   Enable or disable buffering of responses from the origin server. (enterprise plan only)
                               "script_load_optimization": 
                                   Improve the paint time for pages that include JavaScript. (enterprise plan only)
                               "security_header": 
                                   Enforce web security policy for your website.
                               "server_side_exclude": 
                                   Automatically hide specific content from suspicious visitors.
                               "sha1_support": 
                                   Enable support for legacy user agents that do not support certificates signed with modern SHA-2 signatures.
                               "tls_client_auth": 
                                   TLS client certificate presented for authentication on origin pull. (enterprise plan only)
                               "true_client_ip_header": 
                                   CIS will send the end user’s IP address in the True-Client-IP header. (enterprise plan only)
                               "waf": 
                                   A Web Application Firewall (WAF) blocks requests that contain malicious content.
                               "websockets": 
                                   Allow WebSockets connections to your origin server.

   `-v, --value value`      The value set to the feature for domain.
 
                              Valid values for "always_use_https" are "on", "off".
                              Valid values for "automatic_https_rewrites" are "on", "off".
                              Valid values for "browser_check" are "on", "off".
                              Valid values for "challenge_ttl" are "300, 900, 1800, 2700, 3600, 7200, 10800, 14400, 28800, 57600, 86400, 604800, 2592000, 31536000".
                              Valid values for "cname_flattening" are "flatten_all", "flatten_at_root".
                                  "flatten_all": Flatten all CNAME records under your domain.
                                  "flatten_at_root": Flatten CNAME at root domain. This is the default value.
                              Valid values for "hotlink_protection" are "on", "off".
                              Valid values for "http2" are "on", "off".
                              Valid values for "image_load_optimization" are "on", "off".
                              Valid values for "image_size_optimization" are "off", "lossless", "lossy".
                                  "off": Disable Image Size Optimization.
                                  "lossless": Reduce the size of image files without impacting visual quality.
                                  "lossy": The file size of JPEG images is reduced using lossy compression, which may reduce visual quality.
                              Valid values for "ip_geolocation" are "on", "off".
                              Valid values for "ipv6" are "on", "off".
                              Valid values(in MB) for "max_upload" are:
                                  100, 125, 150, 175, 200 and 225, 250, 275, 300, 325, 350, 375, 400, 425, 450, 475, 500 only for Enterprise Plan.
                              Valid values for "min_tls_version" are "1.0", "1.1", "1.2", "1,3".
                              Valid values for "minify" are "css", "html", "js".
                                  "css": Automatically minify all CSS for your website.Valid values for "css" are "on", "off".
                                  "html": Automatically minify all HTML for your website.Valid values for "html" are "on", "off".
                                  "js": Automatically minify all JS for your website.Valid values for "js" are "on", "off".
                                  Eg: -v css=on,html=off,js=on
                              Valid values for "mobile_redirect" are "status", "mobile_subdomain", "strip_uri".
                                  "status": Whether or not the mobile redirection is enabled.Valid values for "status" are "on", "off".
                                  "mobile_subdomain": Which subdomain prefix you wish to redirect visitors on mobile devices to(subdomain must already exist).
                                  "strip_uri": Whether to drop the current page path and redirect to the mobile subdomain URL root.Valid values for "strip_uri" are "true", "false".
                                  Eg: -v status=on,mobile_subdomain=m,strip_uri=true
                              Valid values for "opportunistic_encryption" are "on", "off".
                              Valid values for "origin_error_page_pass_thru" are "on", "off".
                              Valid values for "prefetch_preload" are "on", "off".
                              Valid values for "pseudo_ipv4" are "overwrite_header", "off", "add_header".
                                  "overwrite_header": Overwrite the existing Cf-Connecting-IP and X-Forwarded-For headers with a pseudo IPv4 address.
                                  "off": Disable Pseudo IPv4.
                                  "add_header": Add additional Cf-Pseudo-IPv4 header only.
                              Valid values for "response_buffering" are "on", "off".
                              Valid values for "script_load_optimization" are "on", "off".
                              Valid values for "security_header" are "nosniff", "enabled", "max_age", "include_subdomains", "preload".
                                  "nosniff": Whether or not to send 'X-Content-Type-Options: nosniff' header.Valid values for "nosniff" are "true", "false".
                                  "enabled": Whether or not security_header is enabled.Valid values for "enabled" are "true", "false".
                                  "max_age": Specify the duration(in seconds) security_header are cached in browsers.
                                  "include_subdomains": Every domain below the domain will inherit the same security_header.Valid values for "include_subdomains" are "true", "false".
                                  "preload": Whether or not to permit browsers to preload security_header config.Valid values for "enabled" are "true", "false".
                                  Eg: -v enabled=true,max_age=100,include_subdomains=true,preload=true,nosniff=true
                              Valid values for "server_side_exclude" are "on", "off".
                              Valid values for "sha1_support" are "on", "off".
                              Valid values for "tls_client_auth" are "on", "off".
                              Valid values for "true_client_ip_header" are "on", "off".
                              Valid values for "waf" are "on", "off".
                              Valid values for "websockets" are "on", "off".
   
   `-i, --instance INSTANCE_NAME`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.

   `--output FORMAT`    Specify output format, only JSON is supported now.
  
## Domain
{: #domain}

Manipulate domains by using the following `domain` commands.

### Add domain
{: #add-domain}

**NAME**

   `domain-add` - Add a domain.

**USAGE**

   `ibmcloud cis domain-add DNS_DOMAIN_NAME [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**ARGUMENTS**

   `DNS_DOMAIN_NAME` is the FQDN of DNS domain.

**OPTIONS**

   `-i, --instance INSTANCE_NAME`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.

   `--output FORMAT`    Specify output format, only JSON is supported now.

**Output**
  * ID                     
  * Created On         
  * Modified On  
  * Name                   
  * Original Registrar
  * Origin DNS Host
  * Status                
  * Paused                 
  * Original Name Server
  * Name Servers         


### Resume domain
{: #resume-domain}

**NAME**

   `domain-resume` - Resume the given domain.

**USAGE**

   `ibmcloud cis domain-resume DNS_DOMAIN_ID [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**ARGUMENTS**

   `DNS_DOMAIN_ID` is the ID of DNS domain.

**OPTIONS**

   `-i, --instance INSTANCE_NAME`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.

   `--output FORMAT`    Specify output format, only JSON is supported now.


**Output**
  * ID                     
  * Created On         
  * Modified On  
  * Name                   
  * Original Registrar
  * Origin DNS Host
  * Status                
  * Paused                 
  * Original Name Server
  * Name Servers  
  
### Pause domain
{: #pause-domain}

**NAME**

   `domain-pause` - Pause the given domain.

**USAGE**

   `ibmcloud cis domain-pause DNS_DOMAIN_ID [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**ARGUMENTS**

   DNS_DOMAIN_ID is the ID of DNS domain.

**OPTIONS**

   `-i, --instance INSTANCE_NAME`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.

   `--output FORMAT`    Specify output format, only JSON is supported now.

**Output**
  * ID                     
  * Created On         
  * Modified On  
  * Name                   
  * Original Registrar
  * Origin DNS Host
  * Status                
  * Paused                 
  * Original Name Server
  * Name Servers  

### Display domain
{: #display-domain}

**NAME**

   `domain` - Display the given domain details.

**USAGE**

   `ibmcloud cis domain DNS_DOMAIN_ID [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**ARGUMENTS**

   `DNS_DOMAIN_ID` is the ID of DNS domain.

**OPTIONS**

   `-i, --instance INSTANCE_NAME`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.

   `--output FORMAT`    Specify output format, only JSON is supported now.

**Output**
  * ID                     
  * Created On         
  * Modified On  
  * Name                   
  * Original Registrar
  * Origin DNS Host
  * Status                
  * Paused                 
  * Original Name Server
  * Name Servers 
  
  
### Remove domain
{: #remove-domain}

**NAME**

   `domain-remove` - Remove a domain.

**USAGE**

   `ibmcloud cis domain-remove DNS_DOMAIN_ID [-i, --instance INSTANCE_NAME]`

**ARGUMENTS**

   `DNS_DOMAIN_ID` is the ID of DNS domain.

**OPTIONS**

   `-i, --instance INSTANCE_NAME`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.



### List domains
{: #list-domain}

**NAME**

   `domains` - List domains for a given service instance.

**USAGE**

   `ibmcloud cis domains [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**OPTIONS**

   `-i, --instance INSTANCE_NAME`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.

   `--output FORMAT`    Specify output format, only JSON is supported now.


**Output**
  * ID                      
  * Name                   
  * Status                
  * Paused               


### Domain activation check
{: #check-domain}

**NAME**

   `domain-activation-check` - Perform activation check on the given domain.

**USAGE**

   `ibmcloud cis domain-activation-check DNS_DOMAIN_ID [-i, --instance INSTANCE_NAME]`

**ARGUMENTS**

   `DNS_DOMAIN_ID` is the ID of DNS domain.

**OPTIONS**

   `-i, --instance INSTANCE_NAME`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.
   
## Firewall
{: #firewall}

Manipulate firewalls by using the following `firewall` commands.


### Create Firewall
{: #create-firewall}

**NAME**

   `firewall-create` - Create a new firewall rule.

**USAGE**

   `ibmcloud cis firewall-create (-t, --type Type) (-s, --json-str JSON_STR | -j, --json-file JSON_FILE) [-d --domain DNS_DOMAIN_ID] [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**OPTIONS**

`-t value, --type value`  Type of firewall rule to create. Valid values: `access-rules`, `ua-rules`, `lockdowns`.
  
  * `access-rules`: Access Rules are a way to allow, challenge, or block requests to your website. You can apply access rules to one domain only or all domains in the same service instance.
  * `ua-rules`: Perform access control when matching the exact UserAgent reported by the client. The access control mechanisms can be defined within a rule to help manage traffic from particular clients. This enables you to customize the access to your site.
  * `lockdowns`: Lock access to URLs in this domain to only permitted addresses or address ranges.
    
`-d value, --domain value`    DNS Domain ID.
   
`-s value, --json-str value`  The JSON data describing a firewall rule. Enter `ibmcloud cis help firewall-create -t [type]` for details JSON data format.
   
`-j value, --json-file value`  A file contains input JSON data.
   
`-i, --instance INSTANCE_NAME`   Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.
   
`--output FORMAT`     Specify output format, only JSON is supported now.

#### Create Firewall with access rules
{: #create-firewall-access-rules}

**NAME**

   `firewall-create -t access-rules` - Create a new firewall access rule for a given service instance.

**USAGE**

   `ibmcloud cis firewall-create -t access-rules [-d --domain DNS_DOMAIN_ID] (-s, --json-str JSON_STR | -j, --json-file JSON_FILE) [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**OPTIONS**
   
  `-d value, --domain value`    DNS Domain ID. If not set, this is an instance level firewall access rule.
  
  `-s value, --json-str value`  The JSON data describing a firewall access rule.

The required fields in JSON data are `mode`, `notes`.

 * `mode`: The type of action to perform. Valid values: `block`, `challenge`, `whitelist`, `js_challenge`.
 * `notes`: Some useful information about this rule to help identify the purpose of it.

The optional field is `configuration`.

 * `configuration`: Target/Value pair to use for this rule.
   * `target`: The request property to target. Valid values: "ip", "ip_range", "asn", "country".
   * `value`: The value for the selected target.
     * For `ip` the value is a valid IP address.
     * For `ip_range` the value specifies IP range limited to /16 and /24.
     * For `asn` the value is an AS number.
     * For `country` the value is a country code for the country.

**Sample JSON data:**

```
                   {
                       "mode": "block",
                       "notes": "This rule is added because of event X that occurred on date xyz",
                       "configuration": {
                           "target": "ip",
                           "value": "127.0.0.1"
                       }
                   }
 ```  
   
   `-j value, --json-file value`  A file contains input JSON data.
   
   `-i, --instance INSTANCE_NAME`   Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.
   
   `--output FORMAT`     Specify output format, only JSON is supported now.


#### Create Firewall with user agent rules
{: #create-firewall-agent-rules}

**NAME**

   `firewall-create -t ua-rules` - Create a new user-agent rule for a given domain under a service instance.

**USAGE**

   `ibmcloud cis firewall-create -t ua-rules (-d --domain DNS_DOMAIN_ID) (-s, --json-str JSON_STR | -j, --json-file JSON_FILE) [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**OPTIONS**

   `-d value, --domain value`    DNS Domain ID.
   
   `-s value, --json-str value`  The JSON data describing a user-agent rule.

The required field in JSON data is `mode`.

  * `mode`: The type of action to perform. Valid values: `block`, `challenge`, `js_challenge`.

The optional fields are `paused`, `description`, `configuration`.

  * `paused`: Whether this rule is currently disabled.
  * `description`: Some useful information about this rule to help identify the purpose of it.
  * `configuration`: Target/Value pair to use for this rule.
     * `target`: The request property to target. Valid values: `ua`.
     * `value`: The exact UserAgent string to match with this rule.

**Sample JSON data:**
```
                   {
                       "mode": "block",
                       "configuration": {
                           "target": "ua",
                           "value": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/603.2.4 (KHTML, like Gecko) Version/10.1.1 Safari/603.2.4"
                       }
                   }
  ```                 
                   
   `-j value, --json-file value`  A file contains input JSON data.
   
   `-i, --instance INSTANCE_NAME`  Instance name. If not set, the context instance specified by 'ibmcloud cis instance-set' will be used.
   
   `--output FORMAT`     Specify output format, only JSON is supported now.


#### Create Firewall with lockdowns
{: #create-firewall-lockdowns}

**NAME**

   `firewall-create -t lockdowns` - Create a new lockdown rule for a given zone under a service instance.

**USAGE**

   `ibmcloud cis firewall-create -t lockdowns (-d --domain DNS_DOMAIN_ID) (-s, --json-str JSON_STR | -j, --json-file JSON_FILE) [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**OPTIONS**

   `-d value, --domain value`   DNS Domain ID.
   
   `-s value, --json-str value`  The JSON data describing a user-agent rule.

The optional fields are `paused`, `description`, `urls`, `configurations`.

   * `paused`: Whether this rule is currently disabled.
   * `description`: Some useful information about this rule to help identify the purpose of it.
   * `urls`: URLs included in this rule definition. Wildcards are permitted. The URL pattern entered here is escaped before use. This limits the URL to just simple wildcard patterns.
   * `configurations`: List of IP addresses or CIDR ranges to use for this rule. This can include any number of `ip` or `ip_range` configurations that can access the provided URLs.
     * `target`: The request property to target. Valid values: `ip`, `ip_range`.
     * `value`: IP addresses or CIDR. If target is `ip`, then value is an IP addresses, otherwise CIDR.

**Sample JSON data:**
```
                  {
                       "urls": [
                           "api.mysite.com/some/endpoint*"
                       ],
                       "configurations": [
                           {
                               "target": "ip",
                               "value": "127.0.0.1"
                           },
                           {
                               "target": "ip_range",
                               "value": " 2.2.2.0/24"
                           }
                       ]
                   }
```                   
   `-j value, --json-file value`  A file contains input JSON data.
   
   `-i, --instance INSTANCE_NAME`   Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.
   
   `--output FORMAT`     Specify output format, only JSON is supported now.


### Update Firewall rule
{: #update-firewall}

**NAME**

   `firewall-update` - Update a firewall rule.

**USAGE**

   `ibmcloud cis firewall-update FIREWALL_RULE_ID (-t, --type Type) (-s, --json-str JSON_STR | -j, --json-file JSON_FILE) [-d --domain DNS_DOMAIN_ID] [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**ARGUMENTS**

   `FIREWALL_RULE_ID` is the ID of firewall rule.

**OPTIONS**

`-t value, --type value`  Type of firewall rule to update. Valid values: `access-rules`, `ua-rules`, `lockdowns`.
   
  * `access-rules`: Access Rules are a way to allow, challenge, or block requests to your website. You can apply access rules to one domain only or all domains in the same service instance.
  * `ua-rules`: Perform access control when matching the exact UserAgent reported by the client. The access control mechanisms can be defined within a rule to help manage traffic from particular clients. This enables you to customize the access to your site.
  * `lockdowns`: Lock access to URLs in this domain to only permitted addresses or address ranges.
   
`-d value, --domain value`    DNS Domain ID.
   
`-s value, --json-str value`  The JSON data describing a firewall rule. Enter `ibmcloud cis help firewall-update -t [type]` for details JSON data format.
   
`-j value, --json-file value`  A file contains input JSON data.
   
`-i, --instance INSTANCE_NAME`   Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.
   
`--output FORMAT`     Specify output format, only JSON is supported now.

#### Update Firewalls with access rules
{: #update-firewall-access-rules}

**NAME**

   `firewall-update -t access-rules` - Update a new firewall access rule for a given service instance.

**USAGE**

   `ibmcloud cis firewall-update -t access-rules [-d --domain DNS_DOMAIN_ID] (-s, --json-str JSON_STR | -j, --json-file JSON_FILE) [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**OPTIONS**

   `-d value, --domain value`    DNS Domain ID. If not set, this is an instance level firewall access rule.
   
   `-s value, --json-str value`  The JSON data describing a firewall access rule.

The required fields in JSON data are `mode`, `notes`.

  * `mode`: The type of action to perform. Valid values: `block`, `challenge`, `whitelist`, `js_challenge`.
  * `notes`: Some useful information about this rule to help identify the purpose of it.

The optional field is `configuration`.

  * `configuration`: Target/Value pair to use for this rule.
    * `target`: The request property to target. Valid values: `ip`, `ip_range`, `asn`, `country`.
    * `value`: The value for the selected target.
      *  For ip the value is a valid ip address.
      *  For ip_range the value specifies ip range limited to /16 and /24.
      *  For asn the value is an AS number.
      *  For country the value is a country code for the country.

**Sample JSON data:**

```
                   {
                       "mode": "challenge",
                       "notes": "This rule is added because of event X that occurred on date xyz",
                   }
```   
   `-j value, --json-file value`  A file contains input JSON data.
   
   `-i, --instance INSTANCE_NAME`   Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.
   
   `--output FORMAT`     Specify output format, only JSON is supported now.

#### Update Firewalls with user agent rules
{: #update-firewall-agent-rules}

**NAME**

   `firewall-update -t ua-rules` - Update a new user-agent rule for a given domain under a service instance.

**USAGE**

   `ibmcloud cis firewall-update -t ua-rules (-d --domain DNS_DOMAIN_ID) (-s, --json-str JSON_STR | -j, --json-file JSON_FILE) [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**OPTIONS**

   `-d value, --domain value`    DNS Domain ID.
   
   `-s value, --json-str value`  The JSON data describing a user-agent rule.

The required field in JSON data is `mode`.

  * `mode`: The type of action to perform. Valid values: `block`, `challenge`, `js_challenge`.

The optional fields are `paused`, `description`, `configuration`.

  * `paused`: Whether this rule is currently disabled.
  * `description`: Some useful information about this rule to help identify the purpose of it.
  * `configuration`: Target/Value pair to use for this rule.
    * `target`: The request property to target. Valid values: `ua`.
    * `value`: The exact UserAgent string to match with this rule.
    
**Sample JSON data:**

```
                   {
                       "mode": "block",
                       "configuration": {
                           "target": "ua",
                           "value": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/603.2.4 (KHTML, like Gecko) Version/10.1.1 Safari/603.2.4"
                       }
                   }
```

   `-j value, --json-file value`  A file contains input JSON data.
   
   `-i, --instance INSTANCE_NAME`   Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.
   
   `--output FORMAT`    Specify output format, only JSON is supported now.


#### Update Firewalls with lockdowns
{: #update-firewall-lockdowns}

**NAME**

   `firewall-update -t lockdowns` - Update a new lockdown rule for a given zone under a service instance.

**USAGE**

   `ibmcloud cis firewall-update -t lockdowns (-d --domain DNS_DOMAIN_ID) (-s, --json-str JSON_STR | -j, --json-file JSON_FILE) [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**OPTIONS**

   `-d value, --domain value`    DNS Domain ID.
   
   `-s value, --json-str value`  The JSON data describing a lockdown rule.

The optional fields are `paused`, `description`, `urls`, `configurations`.

  * `paused`: Whether this rule is currently disabled.
  * `description`: Some useful information about this rule to help identify the purpose of it.
  * `urls`: URLs to be included in this rule definition. Wildcards are permitted. The URL pattern entered here will be escaped before use. This limits the URL to just simple wildcard patterns.
  * `configurations`: List of IP addresses or CIDR ranges to use for this rule. This can include any number of ip or ip_range configurations that can access the provided URLs.
    * `target`: The request property to target. Valid values: `ip`, `ip_range`.
    * `value`: IP addresses or CIDR. If target is `ip`, then value should be an IP addresses, otherwise CIDR.

**Sample JSON data:**
```
                   {
                       "urls": [
                           "api.mysite.com/some/endpoint*"
                       ],
                       "configurations": [
                           {
                               "target": "ip",
                               "value": "127.0.0.1"
                           },
                           {
                               "target": "ip_range",
                               "value": " 2.2.2.0/24"
                           }
                       ]
                   }
```

   `-j value, --json-file value`  A file contains input JSON data.
   
   `-i, --instance INSTANCE_NAME`   Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.
   
   `--output FORMAT`     Specify output format, only JSON is supported now.

### List Firewall rules
{: #list-firewall}

**NAME**

   `firewalls` - List firewall rules.

**USAGE**

   `ibmcloud cis firewalls (-t, --type Type) [-d --domain DNS_DOMAIN_ID] [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**OPTIONS**

`-t value, --type value`  Type of firewall rule to list. Valid values: `access-rules`, `ua-rules`, `lockdowns`.
  
  * `access-rules`: Access Rules are a way to allow, challenge, or block requests to your website. You can apply access rules to one domain only or all domains in the same service instance.
  * `ua-rules`: Perform access control when matching the exact UserAgent reported by the client. The access control mechanisms can be defined within a rule to help manage traffic from particular clients. This will enable you to customize the access to your site.
  * `lockdowns`: Lock access to URLs in this domain to only permitted addresses or address ranges.
    
`-d value, --domain value`    DNS Domain ID.
   
`--page value`                Page number of paginated results. (default: 0)
   
`--per-page value`            Maximum number of access rules per page. The minimum value is 5. (default: 20)
   
`-i, --instance INSTANCE_NAME`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.
   
`--output FORMAT`    Specify output format, only JSON is supported now.


### Detail Firewall rule
{: #detail-firewall}

**NAME**

   `firewall` - Get details of a firewall rule settings.

**USAGE**

   `ibmcloud cis firewall FIREWALL_RULE_ID (-t, --type Type) [-d --domain DNS_DOMAIN_ID] [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**ARGUMENTS**

   `FIREWALL_RULE_ID` is the ID of firewall rule.

**OPTIONS**

`-t value, --type value`  Type of firewall settings to check. Valid values: `access-rules`, `ua-rules`, `lockdowns`.
      
  * `access-rules`: Access Rules are a way to allow, challenge, or block requests to your website. You can apply access rules to one domain only or all domains in the same service instance.
  * `ua-rules`: Perform access control when matching the exact UserAgent reported by the client. The access control mechanisms can be defined within a rule to help manage traffic from particular clients. This will enable you to customize the access to your site.
  * `lockdowns`: Lock access to URLs in this domain to only permitted addresses or address ranges.
   
`-d value, --domain value`    DNS Domain ID.
   
`-i, --instance INSTANCE_NAME`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.
   
`--output FORMAT`    Specify output format, only JSON is supported now.

### Delete Firewall
{: #delete-firewall}

**NAME**

   `firewall-delete` - Delete  a firewall rule by ID.

**USAGE**

   `ibmcloud cis firewall-delete FIREWALL_RULE_ID (-t, --type Type) [-d --domain DNS_DOMAIN_ID] [-i, --instance INSTANCE_NAME]`

**ARGUMENTS**

   `FIREWALL_RULE_ID` is the ID of firewall rule.

**OPTIONS**

`-t value, --type value`  Type of firewall rule to delete. Valid values: `access-rules`, `ua-rules`, `lockdowns`.

  * `access-rules`: Access Rules are a way to allow, challenge, or block requests to your website. You can apply access rules to one domain only or all domains in the same service instance.
  * `ua-rules`: Perform access control when matching the exact UserAgent reported by the client. The access control mechanisms can be defined within a rule to help manage traffic from particular clients. This will enable you to customize the access to your site.
  * `lockdowns`: Lock access to URLs in this domain to only permitted addresses or address ranges.
  
   
`-d value, --domain value`    DNS Domain ID.
   
`--output FORMAT`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.

### Get Firewall events
{: #get-firewall}

**NAME**

   `firewall-events` - List events triggered by an active web application firewall rule.

**USAGE**

   `ibmcloud cis firewall-events DNS_DOMAIN_ID [-i, --instance INSTANCE_NAME]  [--output FORMAT]`

**ARGUMENTS**

   `DNS_DOMAIN_ID` is the ID of DNS domain.

**OPTIONS**
   
   `-i, --instance INSTANCE_NAME`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.
   `--output FORMAT`                Specify output format, only JSON is supported now.
   
## Global Load Balancer (GLB)
{: #glb}

Manipulate Global Load Balancers by using the following `glb` commands.  

### Create GLB
{: #create-glb}

**NAME**

   glb-create - Create a global load balancer under a given DNS domain.

**USAGE**

   ibmcloud cis glb-create DNS_DOMAIN_ID (-s, --json-str JSON_STR | -j, --json-file JSON_FILE) [-i, --instance INSTANCE_NAME] [--output FORMAT]

**ARGUMENTS**

   DNS_DOMAIN_ID is the ID of DNS domain.

**OPTIONS**

   -s, --json-str  The JSON data describing a global load balancer.

                   The required fields in JSON data are name, fallback_pool, default_pools:

                       "name":the DNS hostname to associate with your Load Balancer.
                       "fallback_pool":the pool ID to use when all other pools are detected as unhealthy.
                       "default_pools":a list of pool IDs ordered by their failover priority.

                   The optional fields are description, ttl, region_pools, proxied, enabled, session_affinity:

                       "ttl":time to live (TTL) of the DNS entry for the IP address returned by this load balancer.
                       "region_pools":a mapping of region/country codes to a list of pool IDs (ordered by their failover priority) for the given region.
                       "proxied":Control whether or not traffic should flow through the security and performance functions on CIS.
                       "session_affinity":valid values are "cookie", "none".

                   Sample JSON data:

                  {
                       "name": "www.example.com",
                       "fallback_pool": "17b5962d775c646f3f9725cbc7a53df4",
                       "default_pools": [
                           "17b5962d775c646f3f9725cbc7a53df4",
                           "9290f38c5d07c2e2f4df57b1f61d4196"
                       ],
                       "description": "Example global load balancer.",
                       "ttl": 60,
                       "region_pools": {
                           "WNAM": [
                               "de90f38ced07c2e2f4df50b1f61d4194",
                               "9290f38c5d07c2e2f4df57b1f61d4196"
                           ],
                           "ENAM": [
                               "00920f38ce07c2e2f4df50b1f61d4194"
                           ]
                       }
                  }
   -j, --json-file  A file contains input JSON data.
   
   `-i, --instance INSTANCE_NAME`   Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.
   
   `--output FORMAT`     Specify output format, only JSON is supported now.


### Update GLB
{: #update-glb}

**NAME**

   glb-update - Update a global load balancer under a given DNS domain.

**USAGE**

   ibmcloud cis glb-update DNS_DOMAIN_ID GLB_ID (-s, --json-str JSON_STR | -j, --json-file JSON_FILE) [-i, --instance INSTANCE_NAME] [--output FORMAT]

**OPTIONS**

   -s, --json-str  The JSON data describing a global load balancer.

                   The required fields in JSON data are name, fallback_pool, default_pools:

                       "name":the DNS hostname to associate with your Load Balancer.
                       "fallback_pool":the pool ID to use when all other pools are detected as unhealthy.
                       "default_pools":a list of pool IDs ordered by their failover priority.

                   The optional fields are description, ttl, region_pools, proxied, enabled, session_affinity:

                       "ttl":time to live (TTL) of the DNS entry for the IP address returned by this load balancer.
                       "region_pools":a mapping of region/country codes to a list of pool IDs (ordered by their failover priority) for the given region.
                       "proxied":Control whether or not traffic should flow through the security and performance functions on CIS.
                       "session_affinity":valid values are "cookie", "none".

                   Sample JSON data:

                  {
                       "name": "www.example.com",
                       "fallback_pool": "17b5962d775c646f3f9725cbc7a53df4",
                       "default_pools": [
                           "17b5962d775c646f3f9725cbc7a53df4",
                           "9290f38c5d07c2e2f4df57b1f61d4196"
                       ],
                       "description": "Example global load balancer.",
                       "ttl": 60,
                       "region_pools": {
                           "WNAM": [
                               "de90f38ced07c2e2f4df50b1f61d4194",
                               "9290f38c5d07c2e2f4df57b1f61d4196"
                           ],
                           "ENAM": [
                               "00920f38ce07c2e2f4df50b1f61d4194"
                           ]
                       }
                  }
   -j, --json-file  A file contains input JSON data.
   
   `-i, --instance INSTANCE_NAME`   Instance name. If not set, the context instance specified by `ibmcloud  cis instance-set` is used.
   
   `--output FORMAT`     Specify output format, only JSON is supported now.


### Show GLB
{: #show-glb}

**NAME**

   glb - Show a global load balancer under a given DNS domain.

**USAGE**

   ibmcloud cis glb DNS_DOMAIN_ID GLB_ID [-i, --instance INSTANCE_NAME] [--output FORMAT]

**ARGUMENTS**

   DNS_DOMAIN_ID is the ID of DNS domain.
   GLB_ID is the ID of global load balancer.

**OPTIONS**

   `-i, --instance INSTANCE_NAME`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.
   
   `--output FORMAT`    Specify output format, only JSON is supported now.


### Delete GLB
{: #delete-glb}

**NAME**

   glb-delete - Delete a global load balancer under a given DNS domain.

**USAGE**

   ibmcloud cis glb-delete DNS_DOMAIN_ID GLB_ID [-i, --instance INSTANCE_NAME]

**ARGUMENTS**

   DNS_DOMAIN_ID is the ID of DNS domain.
   
   GLB_ID is the ID of global load balancer.

**OPTIONS**

   `-i, --instance INSTANCE_NAME`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.

### List GLB
{: #list-glb}

**NAME**

   glbs - List all load balancers for the given domain.

**USAGE**

   ibmcloud cis glbs DNS_DOMAIN_ID [-i, --instance INSTANCE_NAME] [--output FORMAT]

**ARGUMENTS**

   DNS_DOMAIN_ID is the ID of DNS domain.

**OPTIONS**

   `-i, --instance INSTANCE_NAME`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.
   
   `--output FORMAT`    Specify output format, only JSON is supported now.


### List GLB pools
{: #list-glb-pools}

**NAME**

   glb-pools - List all GLB pools for a given service instance.

**USAGE**

   ibmcloud cis glb-pools [-i, --instance INSTANCE_NAME] [--output FORMAT]

**OPTIONS**

   `-i, --instance INSTANCE_NAME`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.
   
   `--output FORMAT`    Specify output format, only JSON is supported now.


   
### Create GLB pool
{: #create-glb-pool}

**NAME**

   glb-pool-create - Create a GLB pool for a given service instance.

**USAGE**

   ibmcloud cis glb-pool-create (-s, --json-str JSON_STR | -j, --json-file JSON_FILE) [-i, --instance INSTANCE_NAME] [--output FORMAT]

**OPTIONS**

   -s, --json-str  The JSON data used to describe a GLB pool.

                   The required fields in JSON data are name, origins, check_regions:

                       "name":a short name (tag) for the pool.
                       "origins":a list of origins within this pool.
                       "check_regions":a list of geographic region code.

                   The optional fields are description, minimum_origins, enabled, monitor, notification_email:


                   Sample JSON data:

                   {
                       "name": "us-pool",
                       "description": "application server pool in US",
                       "origins": [
                           {
                               "name": "us-app-dal01",
                               "address": "1.1.1.1",
                               "enabled": true
                           },
                           {
                               "name": "us-app-dal02",
                               "address": "2.2.2.2",
                               "enabled": true
                           }
                       ],
                       "minimum_origins": 1,
                       "check_regions": [ "WNAM" ],
                       "monitor": "f1aba936b94213e5b8dca0c0dbf1f9cc",
                       "enabled": true,
                       "notification_email": "someone@example.com"
                   }
   -j, --json-file  A file contains input JSON data.
   
   `-i, --instance INSTANCE_NAME`   Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.
   
   `--output FORMAT`     Specify output format, only JSON is supported now.
   
   
### Show GLB pool
{: #show-glb-pool}

**NAME**

   glb-pool - Show the details of a given GLB pool.

**USAGE**

   ibmcloud cis glb-pool GLB_POOL_ID [-i, --instance INSTANCE_NAME] [--output FORMAT]

**ARGUMENTS**

   GLB_POOL_ID is the ID of global load balancer pool.

**OPTIONS**

   `-i, --instance INSTANCE_NAME`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.
   
   `--output FORMAT`    Specify output format, only JSON is supported now.

   
### Delete GLB pool
{: #delete-glb-pool}

**NAME**

   glb-pool-delete - Delete a given GLB pool.

**USAGE**

   ibmcloud cis glb-pool-delete GLB_POOL_ID [-i, --instance INSTANCE_NAME]

**ARGUMENTS**

   GLB_POOL_ID is the ID of global load balancer pool.

**OPTIONS**

   `-i, --instance INSTANCE_NAME`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.
   
### Update GLB pool
{: #update-glb-pool}

**NAME**

   glb-pool-update - Update the details of a given GLB pool.

**USAGE**

   ibmcloud cis glb-pool-update GLB_POOL_ID [-s, --json-str JSON_STR | -j, --json-file JSON_FILE] [--enable-origin ORIGIN_NAME --enable-origin ORIGIN_NAME ...] [--disable-origin ORIGIN_NAME --disable-origin ORIGIN_NAME ...] [--add-origin ORIGIN_PARAMETER --add-origin ORIGIN_PARAMETER ...] [--remove-origin ORIGIN_NAME --remove-origin ORIGIN_NAME ...] [-i, --instance INSTANCE_NAME] [--output FORMAT]


**ARGUMENTS**

   GLB_POOL_ID is the ID of global load balancer pool.

**OPTIONS**

   -s, --json-str  The JSON data used to describe a GLB pool.

                   The required fields in JSON data are name, origins, check_regions:

                       "name":a short name (tag) for the pool.
                       "origins":a list of origins within this pool.
                       "check_regions":a list of geographic region code.

                   The optional fields are description, minimum_origins, enabled, monitor, notification_email:


                   Sample JSON data:

                   {
                       "name": "us-pool",
                       "description": "application server pool in US",
                       "origins": [
                           {
                               "name": "us-app-dal01",
                               "address": "1.1.1.1",
                               "enabled": true
                           },
                           {
                               "name": "us-app-dal02",
                               "address": "2.2.2.2",
                               "enabled": true
                           }
                       ],
                       "minimum_origins": 1,
                       "check_regions": [ "WNAM" ],
                       "monitor": "f1aba936b94213e5b8dca0c0dbf1f9cc",
                       "enabled": true,
                       "notification_email": "someone@example.com"
                   }
   -j, --json-file  A file contains input JSON data.
   
   --enable-origin value        Enable the origin within the Pool. The value can be ORIGIN_NAME or ORIGIN_ADDRESS.
   
   --disable-origin value       Disable the origin within the Pool. The value can be ORIGIN_NAME or ORIGIN_ADDRESS.
   
   --add-origin value           Add an origin into the Pool. ORIGIN_NAME and ORIGIN_ADDRESS are required.
                                Eg: --add-origin name=us-app-dal01,address=1.1.1.1,enabled=true,weight=0.5
                                
   --remove-origin value       Remove an origin from the Pool. The value can be ORIGIN_NAME or ORIGIN_ADDRESS.
   
   `-i, --instance INSTANCE_NAME`   Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.
   
   `--output FORMAT`     Specify output format, only JSON is supported now.

   
### List GLB monitors 
{: #list-glb-monitors}

**NAME**

   glb-monitors - List GLB monitors for a given service instance.

**USAGE**

   ibmcloud cis glb-monitors [-i, --instance INSTANCE_NAME] [--output FORMAT]

**OPTIONS**

   -i, --instance  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.
   
   `--output FORMAT`    Specify output format, only JSON is supported now.

   
### Create GLB monitor
{: #create-glb-monitor}

**NAME**

   glb-monitor-create - Create a GLB monitor for a given service instance.

**USAGE**

   ibmcloud cis glb-monitor-create (-s, --json-str JSON_STR | -j, --json-file JSON_FILE) [-i, --instance INSTANCE_NAME] [--output FORMAT]

**OPTIONS**

   -s, --json-str  The JSON data used to describe a GLB monitor.

                   The required fields in JSON data are "type".

                       "type": The protocol to use for the healthcheck. Valid values: "HTTP", "HTTPS", "TCP".

                   The optional fields are "description", "timeout", "retries", "interval".

                       "description": Description.
                       "timeout": The timeout (in seconds) before marking the health check as failed.
                       "retries": The number of retries to attempt in case of a timeout before marking the origin as unhealthy.
                       "interval": The interval between each health check.

                   For "TCP" type health check:

                       Extra required fields are "port".

                           "port": The TCP port to use for the health check.

                   For "HTTP/HTTPS" type health check:

                       Extra option fields are "port", "expected_body", "expected_codes", "method", "path", "header", "follow_redirects", "allow_insecure".

                           "port": The TCP port to use for the health check.
                           "expected_body": A case-insensitive sub-string to look for in the response body.
                           "expected_codes": The expected HTTP response code or code range of the health check.
                           "method": The HTTP method to use for the health check.
                           "path": The endpoint path to health check against.
                           "header": The HTTP request headers to send in the health check.
                           "follow_redirects": Follow redirects if returned by the origin.
                           "allow_insecure": Do not validate the certificate when monitor use HTTPS.


                   Sample JSON data:

                       For HTTP/HTTPS:

                           {
                               "description": "Health monitor of web service",
                               "type": "https",
                               "method": "GET",
                               "path": "/health",
                               "header": {
                                   "Host": [
                                       "example.com"
                                   ],
                                   "X-App-ID": [
                                       "abc123"
                                   ]
                               },
                               "timeout": 5,
                               "retries": 2,
                               "interval": 90,
                               "follow_redirects": true,
                               "allow_insecure": false,
                               "expected_codes": "2xx",
                               "expected_body": "alive"
                           }

                       For TCP:

                           {
                               "description": "Health monitor of TCP",
                               "type": "tcp",
                               "port": 80,
                               "timeout": 5,
                               "retries": 2,
                               "interval": 90
                           }

   -j, --json-file  A file contains input JSON data.
   
   `-i, --instance INSTANCE_NAME`   Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.
   
   `--output FORMAT`     Specify output format, only JSON is supported now.

   
### Show GLB monitor 
{: #show-glb-monitor}

**NAME**

   glb-monitor - Show the GLB monitor for a given service instance.

**USAGE**

   ibmcloud cis glb-monitor GLB_MON_ID [-i, --instance INSTANCE_NAME] [--output FORMAT]

**ARGUMENTS**

   GLB_MON_ID is the ID of global load balancer monitor.

**OPTIONS**

   `-i, --instance INSTANCE_NAME`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used. 
   
   `--output FORMAT`    Specify output format, only JSON is supported now.

   
### Delete GLB Monitor
{: #delete-glb-monitor}
**NAME**

   glb-monitor-delete - Delete the GLB monitor for a given service instance.

**USAGE**

   ibmcloud cis glb-monitor-delete GLB_MON_ID [-i, --instance INSTANCE_NAME]

**ARGUMENTS**

   GLB_MON_ID is the ID of global load balancer monitor.

**OPTIONS**

   `-i, --instance INSTANCE_NAME`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.


### Update GLB monitor
{: #update-glb-monitor}

**NAME**

   glb-monitor-update - Update the GLB monitor for a given service instance.

**USAGE**

   ibmcloud cis glb-monitor-update GLB_MON_ID (-s, --json-str JSON_STR | -j, --json-file JSON_FILE) [-i, --instance INSTANCE_NAME] [--output FORMAT]

**ARGUMENTS**

   GLB_MON_ID is the ID of global load balancer monitor.

**OPTIONS**

   -s, --json-str  The JSON data used to describe a GLB monitor.

                   The required fields in JSON data are "type".

                       "type": The protocol to use for the healthcheck. Valid values: "HTTP", "HTTPS", "TCP".

                   The optional fields are "description", "timeout", "retries", "interval".

                       "description": Description.
                       "timeout": The timeout (in seconds) before marking the health check as failed.
                       "retries": The number of retries to attempt in case of a timeout before marking the origin as unhealthy.
                       "interval": The interval between each health check.

                   For "TCP" type health check:

                       Extra required fields are "port".

                           "port": The TCP port to use for the health check.

                   For "HTTP/HTTPS" type health check:

                       Extra option fields are "port", "expected_body", "expected_codes", "method", "path", "header", "follow_redirects", "allow_insecure".

                           "port": The TCP port to use for the health check.
                           "expected_body": A case-insensitive sub-string to look for in the response body.
                           "expected_codes": The expected HTTP response code or code range of the health check.
                           "method": The HTTP method to use for the health check.
                           "path": The endpoint path to health check against.
                           "header": The HTTP request headers to send in the health check.
                           "follow_redirects": Follow redirects if returned by the origin.
                           "allow_insecure": Do not validate the certificate when monitor use HTTPS.


                   Sample JSON data:

                       For HTTP/HTTPS:

                           {
                               "description": "Health monitor of web service",
                               "type": "https",
                               "method": "GET",
                               "path": "/health",
                               "header": {
                                   "Host": [
                                       "example.com"
                                   ],
                                   "X-App-ID": [
                                       "abc123"
                                   ]
                               },
                               "timeout": 5,
                               "retries": 2,
                               "interval": 90,
                               "follow_redirects": true,
                               "allow_insecure": false,
                               "expected_codes": "2xx",
                               "expected_body": "alive"
                           }

                       For TCP:

                           {
                               "description": "Health monitor of TCP",
                               "type": "tcp",
                               "port": 80,
                               "timeout": 5,
                               "retries": 2,
                               "interval": 90
                           }
   -j, --json-file  A file contains input JSON data.
   
   `-i, --instance INSTANCE_NAME`   Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.
   
   `--output FORMAT`     Specify output format, only JSON is supported now.


### Get GLB events
{: #get-glb-events}

**NAME**

   glb-events - List status changes from origins connected to a GLB monitor.

**USAGE**

   ibmcloud cis glb-events [-s, --since START_DATE] [-u, --until END_DATE] [--origin-name ORIGIN_NAME] [--pool-name POOL_NAME] 
                           [--origin-healthy (true | false)] [--pool-healthy (true | false)]
                           [-i, --instance INSTANCE_NAME]  [--output FORMAT]

**ARGUMENTS**
  
**OPTIONS**

   -s, --since       Start date requesting data period in the ISO8601 format. For example `2018-11-26`.

   -u, --until       End date requesting data period in the ISO8601 format. For example `2018-11-28`.

   --origin-name     The name for the origin to filter for.

   --pool-name       The name for the pool to filter for.

   --origin-healthy  If true, filter events where the origin status is healthy, if false, filter events where the origin status is unhealthy. 
                     Default value is `true`,  valid values are `true` and `false`.

   --pool-healthy    If true, filter events where the pool status is healthy, if false, filter events where the pool status is unhealthy. 
                     Default value is `true`,  valid values are `true` and `false`.

   `-i, --instance INSTANCE_NAME`    Instance name. If not set, the context instance specified by `ibmcloudcis instance-set` is used.
   
   `--output FORMAT`      Output the result as JSON style to a file. If not set, outputs the result to terminal.


## Logpull
{: #log}

Manipulate Logpull services by using the following `logpull` commands. 


### Retrieve edge http logs
{: #retrieve-log}

**NAME**

   `logpull` - Retrieve edge http logs for a given DNS domain. (enterprise plan only)

**USAGE**

   `ibmcloud cis logpull DNS_DOMAIN_ID --available-fields`

   `ibmcloud cis logpull DNS_DOMAIN_ID --ray-id  RAY_ID [--fields FIELD1,FIELD2,FIELD3...]`

   `ibmcloud cis logpull DNS_DOMAIN_ID [--start START] [--end END] [--fields FIELD1,FIELD2,FIELD3...] [--count COUNT] [--sample SAMPLE]`


**ARGUMENTS**

   `DNS_DOMAIN_ID` is the id of DNS domain.

**OPTIONS**

   `--available-fields`          List of all available fields.

   `--ray-id value`              Lookup logs by specific Ray ID.

   `--fields value`              Define field set in return.
                              This must be specified as a comma separated list without any whitespaces, and all fields must exist.
                               The order in which fields are specified doesn't matter, and the order of fields in the response is not specified.
                               Note that fields are expected to be case sensitive.

   `--start value`               The (inclusive) beginning of the requested time frame.
                               This can be a unix timestamp (in seconds or nanoseconds), or an absolute timestamp that conforms to RFC 3339.
                               At this point in time, it cannot exceed a time in the past greater than 7 days.
                               Default is 65 minutes earlier.

   `--end value`                 The (exclusive) end of the requested time frame.
                               This can be a unix timestamp (in seconds or nanoseconds), or an absolute timestamp that conforms to RFC 3339.
                               The `end` must be at least 5 minutes earlier than now and must be later than `start`.
                               Difference between 'start' and 'end' must be not greater than 1h.
                               Default is 5 minutes earlier.

  `--count value`               Number of logs to retrieve. (default: -1)
  
   `--sample value`              Percentage of sampling. When sample is provided, a sample of matching records is returned.
                               If sample=0.1 then 10% of records will be returned.
                               Sampling is random: repeated calls will not only return different records, but likely will also vary slightly in number of returned records.
                               When count is also specified, count is applied to the number of returned records, not the sampled records.
                               So, with sample=0.05 and count=7, when there is a total of 100 records available, approximately 5 will be returned.
                               When there are 1000 records, 7 will be returned. When there are 10,000 records, 7 will be returned. (default: 1)

   `-i, --instance INSTANCE_NAME`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.
   
## Metrics
{: #metrics}

Manipulate metrics by using the following `metrics` commands.

### Web Analytics
{: #web-analytics}

**NAME**

   `web-analytics` - Get analytics of the DNS domain.

**USAGE**

   `ibmcloud cis web-analytics DNS_DOMAIN_ID [--recent DURATION] [-t, --table requests | bandwidth | uniques | threats | status_code] [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**ARGUMENTS**

   `DNS_DOMAIN_ID` is the ID of DNS domain.

**OPTIONS**

   `--recent`  The beginning of the requested time frame. Valid vaules are: 6h (6 hours ago), 12h, 1d (1 day ago), 1w (1 week ago), 1m (1 month ago), 2m, 3m. (default: `1w`)

   `-t, --table`     Output table. Valid values are `requests`, `bandwidth`, `uniques`, `threats` and `status_code`. If not set, it outputs all the tables.

   `-i, --instance INSTANCE_NAME`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.

   `--output FORMAT`    Specify output format, only JSON is supported now.


### DNS analytics
{: #dns-analytics}

**NAME**

   `dns-analytics` - Get DNS analytics of the domain.

**USAGE**

   `ibmcloud cis dns-analytics DNS_DOMAIN_ID DIMENSION [-i, --instance INSTANCE_NAME] [-s, --since TIME] [--output FORMAT]`

**ARGUMENTS**

   `DNS_DOMAIN_ID` is the ID of DNS domain.

   `DIMENSION` is queried dimension. The valid value is either `queries-by-response-code` or `queries-by-type`.

**OPTIONS**

   `-i, --instance INSTANCE_NAME`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.

   `-s, --since` Since time to now. Valid values are: `6h` (6 hours ago), `12h`, `1d` (1 day ago), `1w` (1 week ago)

   `--output FORMAT`  Specify output format, only JSON is supported now.


### Ratelimit Analytics
{: #ratelimit-analytics}

**NAME**

   `ratelimit-analytics` - Get rate limit analytics for a DNS domain.

**USAGE**

   `ibmcloud cis ratelimit-analytics DNS_DOMAIN_ID [--recent DURATION] [--time-delta SECONDS] [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**ARGUMENTS**

   `DNS_DOMAIN_ID` is the ID of DNS domain.

**OPTIONS**
   `--recent`             The beginning of the requested time frame. Valid vaules are: 6h (6 hours ago), 12h, 1d (1 day ago), 1w (1 week ago), 1m (1 month ago), 2m, 3m. (default: `1w`)

   `--time-delta`  The time interval (seconds) of each analytic's record. Valid values: `60`, `3600`, `86400`, `2592000`. (default: 3600)

   `-i, --instance INSTANCE_NAME` Instance name. If not set, the context instance specified by 'ibmcloud cis instance-set' will be used.

   `--output FORMAT`   Specify output format, only JSON is supported now.


## Pagerule

Manipulate page rules by using the following `pagerule` commmands.

### Create Page Rule
{: #create-page-rule}

**NAME**

   page-rule-create - Create a page rule of the DNS domain.

**USAGE**

   ibmcloud cis page-rule-create DNS_DOMAIN_ID (-s, --json-str JSON_STR | -j, --json-file JSON_FILE) [-i, --instance INSTANCE_NAME] [--output FORMAT]

**ARGUMENTS**

   DNS_DOMAIN_ID is the ID of DNS domain.

**OPTIONS**

   -s, --json-str  The JSON data describing a page rule.

                   The required fields in JSON data are targets, actions:

                       "targets":The target URL pattern to evaluate on a request.
                       "actions":An array of actions to perform if the targets of this rule match the request. Available actions are:
                           disable_security, always_use_https, ssl, browser_cache_ttl,
                           security_level, cache_level, edge_cache_ttl, bypass_cache_on_cookie,
                           browser_check, server_side_exclude, serve_stale_content, email_obfuscation,
                           automatic_https_rewrites, opportunistic_encryption, ip_geolocation,
                           explicit_cache_control, cache_deception_armor, waf, forwarding_url,
                           host_header_override, resolve_override.


                           And some actions are limited to Enterprise plan:
                               cache_on_cookie, disable_apps, disable_performance, minify,
                               image_load_optimization, origin_error_page_pass_thru, response_buffering,
                               image_size_optimization, script_load_optimization, true_client_ip_header,
                               sort_query_string_for_cache.

                   The optional fields are priority, status:

                       "priority":A number that indicates the preference for a page rule over another. Default is 1.
                       "status":Status of the page rule. The valid values are 'active' and 'disabled', default is 'disabled'.

                   Sample JSON data:

                   {
                       "targets": [
                           {
                               "target": "url",
                               "constraint": {
                                   "operator": "matches",
                                   "value": "*example.com/images/*"
                               }
                           }
                       ],
                       "actions": [
                           {
                               "id": "serve_stale_content",
                               "value": "on"
                           },
                           {
                               "id": "ssl",
                               "value": "flexible"
                           },
                           {
                               "id": "browser_cache_ttl",
                               "value": 14400
                           },
                           {
                               "id": "security_level",
                               "value": "medium"
                           },
                           {
                               "id": "cache_level",
                               "value": "basic"
                           },
                           {
                               "id": "edge_cache_ttl",
                               "value": 7200
                           },
                           {
                               "id": "bypass_cache_on_cookie",
                               "value": "wp-.*|wordpress.*|comment_.*"
                           }
                       ]
                   }
                   
   -j, --json-file  A file contains input JSON data.
   
   `-i, --instance INSTANCE_NAME`   Instance name. If not set, the context instance specified by 'ibmcloud cis instance-set' is used.
   
   `--output FORMAT`     Specify output format, only JSON is supported now.


### Update a Page Rule
{: #update-page-rule}

**NAME**

   page-rule-update - Update the page rule of the DNS domain.

**USAGE**

   ibmcloud cis page-rule-update DNS_DOMAIN_ID PAGE_RULE_ID (-s, --json-str JSON_STR | -j, --json-file JSON_FILE) [-i, --instance INSTANCE_NAME] [--output FORMAT]

**ARGUMENTS**

   DNS_DOMAIN_ID is the ID of DNS domain.
   
   PAGE_RULE_ID is the ID of page rule.

**OPTIONS**

   -s, --json-str  The JSON data describing a page rule.

                   The required fields in JSON data are targets, actions:

                       "targets":The target URL pattern to evaluate on a request.
                       "actions":An array of actions to perform if the targets of this rule match the request. Available actions are:
                           disable_security, always_use_https, ssl, browser_cache_ttl,
                           security_level, cache_level, edge_cache_ttl, bypass_cache_on_cookie,
                           browser_check, server_side_exclude, serve_stale_content, email_obfuscation,
                           automatic_https_rewrites, opportunistic_encryption, ip_geolocation,
                           explicit_cache_control, cache_deception_armor, waf, forwarding_url,
                           host_header_override, resolve_override.

                           And some actions are limited to Enterprise plan:
                               cache_on_cookie, disable_apps, disable_performance, minify,
                               image_load_optimization, origin_error_page_pass_thru, response_buffering,
                               image_size_optimization, script_load_optimization, true_client_ip_header,
                               sort_query_string_for_cache.

                   The optional fields are priority, status:

                       "priority":A number that indicates the preference for a page rule over another. Default is 1.
                       "status":Status of the page rule. The valid values are 'active' and 'disabled', default is 'disabled'.

                   Sample JSON data:

                   {
                       "targets": [
                           {
                               "target": "url",
                               "constraint": {
                                   "operator": "matches",
                                   "value": "*example.com/images/*"
                               }
                           }
                       ],
                       "actions": [
                           {
                               "id": "serve_stale_content",
                               "value": "on"
                           },
                           {
                               "id": "ssl",
                               "value": "flexible"
                           },
                           {
                               "id": "browser_cache_ttl",
                               "value": 14400
                           },
                           {
                               "id": "security_level",
                               "value": "medium"
                           },
                           {
                               "id": "cache_level",
                               "value": "basic"
                           },
                           {
                               "id": "edge_cache_ttl",
                               "value": 7200
                           },
                           {
                               "id": "bypass_cache_on_cookie",
                               "value": "wp-.*|wordpress.*|comment_.*"
                           }
                       ]
                   }
                   
   -j, --json-file  A file contains input JSON data.
   
   `-i, --instance INSTANCE_NAME`   Instance name. If not set, the context instance specified by 'ibmcloud cis instance-set' is used.
   
   `--output FORMAT`     Specify output format, only JSON is supported now.


### Delete a Page Rule
{: #delete-page-rule}

**NAME**

   page-rule-delete - Delete a page rule of the DNS domain.

**USAGE**

   ibmcloud cis page-rule-delete DNS_DOMAIN_ID PAGE_RULE_ID [-i, --instance INSTANCE_NAME]

**ARGUMENTS**

   DNS_DOMAIN_ID is the ID of DNS domain.
   
   PAGE_RULE_ID is the ID of page rule.

**OPTIONS**

   `-i, --instance INSTANCE_NAME`  Instance name. If not set, the context instance specified by 'ibmcloud cis instance-set' is used.


### List Page Rules
{: #list-page-rule}

**NAME**

   page-rules - List page rules of the DNS domain.

**USAGE**

   ibmcloud cis page-rules DNS_DOMAIN_ID [-i, --instance INSTANCE_NAME] [--output FORMAT]

**ARGUMENTS**

   DNS_DOMAIN_ID is the ID of DNS domain.

**OPTIONS**

   `-i, --instance INSTANCE_NAME`  Instance name. If not set, the context instance specified by 'ibmcloud cis instance-set' is used.
   
   `--output FORMAT`    Specify output format, only JSON is supported now.


### Show a Page Rule
{: #show-page-rule}

**NAME**

   page-rule - Get details of a page rule.

**USAGE**

   ibmcloud cis page-rule DNS_DOMAIN_ID PAGE_RULE_ID [-i, --instance INSTANCE_NAME] [--output FORMAT]

**ARGUMENTS**

   DNS_DOMAIN_ID is the ID of DNS domain.
   
   PAGE_RULE_ID is the ID of page rule.

**OPTIONS**

   `-i, --instance INSTANCE_NAME`  Instance name. If not set, the context instance specified by 'ibmcloud cis instance-set' is used.
   
   `--output FORMAT`    Specify output format, only JSON is supported now.
   
## Rate Limiting
{: #ratelimit}

Manipulate rate limits by using the following `ratelimit` commands.


### Create Rate Limit rule
{: #create-ratelimit}

**NAME**

    `ratelimit-rule-create` - Create a new rate limiting rule for a given DNS domain. (enterprise plan only)

**USAGE**

   `ibmcloud cis ratelimit-rule-create  DNS_DOMAIN_ID (-s, --json-str JSON_STR | -j, --json-file JSON_FILE) [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**ARGUMENTS**

   `DNS_DOMAIN_ID` is the ID of DNS domain.

**OPTIONS**

   `-s, --json-str`  The JSON data describing a rate limit rule.

The required fields in JSON data are `match`, `threshold`, `period`, `action`:

 * `match`: Determines which traffic the rate limiting rule counts towards the threshold.
   * `request`: Matches HTTP requests.
     * `methods`:  HTTP Methods, can be a subset [`POST`,`PUT`] or all [`_ALL_`]. This field is not required to create a  
                                                             rate limit rule. Valid values are `GET`, `POST`, `PUT`, `DELETE`, `PATCH`, `HEAD`, `_ALL_`.
     * `schemes`:  HTTP Schemes, can be one [`HTTPS`], both [`HTTP`,`HTTPS`] or all [`_ALL_`]. This field is not required.
     * `url`: The URL pattern to match comprised of the host and path, for instance, example.org/path. Wildcards are expanded to match applicable traffic, query strings are not matched. Use `*` for all traffic to your zone. Max length is 1024.
   * `response`: Matches HTTP responses before they are returned to the client . If this is defined, then the entire counting of traffic occurs at this stage. This field is not required.
     * `status`: HTTP Status codes, can be one [403], many [401,403] or indicate all by not providing this value. This field is not required. Min value: 100, max value: 999.
     * `headers`: Array of response headers to match. If a response does not meet the header criteria then the request is not counted towards the rate limiting rule. The header matching criteria includes following properties.
       * `name`: The name of the response header to match.
       * `op`: The operator when matching, eq means equals, ne means not equals. Valid values are `eq` and `ne`.
       * `value`: The value of the header, which is exactly matched.
   * `threshold`: The threshold that triggers the rate limit mitigations, combined with period. For example, threshold per period. Min value: 2, max value: 1000000.                       
   * `period`: The time, in seconds, to count matching traffic. If the count exceeds threshold within this period the action is performed. Min value:1, max value: 3600.
   * `action`: The action performed when the threshold of matched traffic within the period defined is exceeded.
     * `mode`: The type of action performed. Valid values are: `simulate`, `ban`, `challenge`, `js_challenge`.
     * `timeout`: The time, in seconds, as an integer to perform the mitigation action. Timeout be the same or greater than the period. This field is valid only when mode is `simulate` or `ban`. Min value: 10, max value: 86400.
     * `response`: Custom content-type and body to return. This overrides the custom error for the zone. This field is not required. Omission results in the default HTML error page. This field is valid only when mode is `simulate` or `ban`.
       * `content_type`: The content-type of the body, which must be one of the following: `text/plain`, `text/xml`, `application/json`.
       * `body`: The body to return. The content here must conform to the `content_type`. Max length is 10240.

The optional fields are `id`, `disabled`, `description`, and `bypass`:
      
   * `id`: Identifier of the rate limiting rule.
   * `disabled`: Whether this rate limiting rule is currently disabled.
   * `description`: A note that you can use to describe the reason for a rate limiting rule.
   * `bypass`: Criteria that allows the rate limit to be bypassed. For example, to express that you shouldn’t apply a rate limit to a given set of URLs.
     * `name`: Valid values is `url`.
     * `value`: The url to bypass.

**Sample JSON data**:
```
                          {
                                  "id": "92f17202ed8bd63d69a66b86a49a8f6b",
                                  "disabled": false,
                                  "description": "Prevent multiple login failures to mitigate brute force attacks",
                                  "bypass": [
                                          {
                                                  "name": "url",
                                                  "value": "api.example.com/*"
                                          }
                                    ],
                                    "threshold": 60,
                                    "period": 900,
                                    "action": [
                                            {
                                                   "mode": "simulate",
                                                   "timeout": 86400,
                                                   "response": {
                                                           "content_type": "text/plain",
                                                           "body": "<error>This request has been rate-limited.</error>"
                                                   }
                                             }
                                     ],
                                     "match": {
                                             "request": {
                                                     "methods": [
                                                             "GET"
                                                      ],
                                                      "schemes": [
                                                             "HTTP",
                                                             "HTTPS"
                                                      ],
                                                      "url": "*.example.org/path*"
                                              },
                                              "response": {
                                                       "status": [
                                                               403, 401
                                                        ],
                                                       "headers": [
                                                                 {
                                                                         "name": "Cf-Cache-Status",
                                                                         "op": "eq",
                                                                         "value": "HIT"
                                                                  }
                                                        ]
                                              }
                                     }
                          }
```

   `-j, --json-file` A file contains input JSON data.
   
   `-i, --instance INSTANCE_NAME` Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.
   
   `--output FORMAT` Specify output format, only JSON is supported now.

### Update Rate Limit rule
{: #update-ratelimit}

**NAME**

   `ratelimit-rule-update` - update a rate limiting rule of a given DNS domain.

**USAGE**

   `ibmcloud cis ratelimit-rule-update DNS_DOMAIN_ID RATELIMIT_RULE_ID  (-s, --json-str JSON_STR | -j, --json-file JSON_FILE) [-i, --instance INSTANCE_NAME] [--output FORMAT]`


**ARGUMENTS**

   `DNS_DOMAIN_ID` is the ID of DNS domain.
   
   `RATELIMIT_RULE_ID` is the ID of rate limiting rule. 

**OPTIONS**

   `-s, --json-str`  The JSON data describing a rate limit rule.

The required fields in JSON data are `id`, `match`, `threshold`, `period`, `action`:

  * `id`: Identifier of the rate limiting rule.
  * `match`: Determines which traffic the rate limit counts towards the threshold.
    * `request`: Matches HTTP requests.
      * `methods:  HTTP Methods, can be a subset [`POST`,`PUT`] or all [`_ALL_`]. This field is not required to create a rate limit rule. Valid values are `GET`, `POST`, `PUT`, `DELETE`, `PATCH`, `HEAD`, `_ALL_`.
      * `schemes`:  HTTP Schemes, can be one [`HTTPS`], both [`HTTP`,`HTTPS`] or all [`_ALL_`]. This field is not required.
      * `url: The URL pattern to match comprised of the host and path, for example, example.org/path. Wildcards are expanded to match applicable traffic, query strings are not matched. Use `*` for all traffic to your zone. Max length is 1024.
    * `response`: Matches HTTP responses before they are returned to the client . When defined, the entire counting of traffic occurs at this stage. This field is not required.
      * `status`: HTTP Status codes, can be one [403], many [401,403] or indicate all by not providing this value. This field is not required. Min value: 100, max value: 999.
      * `headers`: Array of response headers to match. If a response does not meet the header criteria then the request is not counted towards the rate limit. The header matching criteria includes following properties.
        * `name`: The name of the response header to match.
        * `op`: The operator when matching, eq means equals, ne means not equals. Valid values are `eq` and `ne`.
        * `value`: The value of the header, which is exactly matched.
  * `threshold`: The threshold that triggers the rate limit mitigations, combined with period. For example, threshold per period. Min value: 2, max value: 1000000.                       
  * `period`: The time, in seconds, to count matching traffic. If the count exceeds threshold within this period the action is performed. Min value:1, max value: 3600.
  * `action`: The action to be performed when the threshold of matched traffic within defined period is exceeded.
    * `mode`: The type of action performed. Valid values are: `simulate`, `ban`, `challenge`, `js_challenge`.
    * `timeout`: The time, in seconds as an integer, to perform the mitigation action. Timeout must be the same or greater than the period. This field is only valid when mode is `simulate` or `ban`. Min value: 10, max value: 86400.
    * `response`: Custom content-type and body to return. This overrides the custom error for the zone. This field is not required. Omission results in default HTML error page. This field is only valid when mode is `simulate` or `ban`.
      * `content_type`: The content-type of the body. Must be one of the following: `text/plain`, `text/xml`, `application/json`.
      * `body`: The body to return. The content here must conform to the content_type. Max length is 10240.

The optional fields are `disabled`, `description`, and `bypass`:      
                    
 * `disabled`: Whether this rate limiting rule is currently disabled.
 * `description`: A note that you can use to describe the reason for a rate limit.
 * `bypass`: Criteria that allows the rate limit to be bypassed. For example, to express that you shouldn’t apply a rate limit to a given set of URLs.
    * `name`: Valid values is `url`.
    * `value`: The url to bypass.

  `-j, --json-file` A file contains input JSON data.
  
  `-i, --instance INSTANCE_NAME` Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.
  
  `--output FORMAT` Specify output format, only JSON is supported now.


### List Rate Limit rules
{: #list-ratelimit}

**NAME**

   `ratelimit-rules` - List rate limiting rules of a given DNS domain.

**USAGE**

   `ibmcloud cis ratelimit-rules  DNS_DOMAIN_ID [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**ARGUMENTS**

   `DNS_DOMAIN_ID` is the ID of DNS domain.

**OPTIONS**

  `-i, --instance INSTANCE_NAME`   Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.
  
  `--output FORMAT`     Specify output format, only JSON is supported now.



### Detail Rate Limit rule
{: #detail-ratelimit}

**NAME**

   `ratelimit-rule` - Get details of a rate limiting rule by ID.

**USAGE**

   `ibmcloud cis ratelimit-rule DNS_DOMAIN_ID  RATELIMIT_RULE_ID [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**ARGUMENTS**

   `DNS_DOMAIN_ID` is the ID of DNS domain.
   
   `RATELIMIT_RULE_ID` is the ID of rate limit rule. 

**OPTIONS**

   `-i, --instance INSTANCE_NAME`   Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.
   
   `--output FORMAT`     Specify output format, only JSON is supported now.


### Delete Rate Limit rule
{: #delete-ratelimit}

**NAME**

   `ratelimit-rule-delete` - Delete a rate limiting rule by ID.

**USAGE**

   `ibmcloud cis ratelimit-rule-delete DNS_DOMAIN_ID RATELIMIT_RULE_ID [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**ARGUMENTS**

   `DNS_DOMAIN_ID` is the ID of DNS domain.
   
   `RATELIMIT_RULE_ID` is the ID of rate limit rule. 

**OPTIONS**

   `-i, --instance INSTANCE_NAME`   Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.
   
   `--output FORMAT`     Specify output format, only JSON is supported now.
   
## Resource Instance
{: #resource-instance}

Manipulate CIS Service instances by using the following `instance` commands.

### List CIS Service Instances
{: #list-cis-service-instances}

**NAME**

   `instances` - List all CIS service instances.

**USAGE**

   `ibmcloud cis instances`

**Output**
   * Name
   * Location
   * State
   * Service Name


### Set Context CIS Service Instance
{: #set-context-cis-service-instance}

**NAME**

   `instance-set` - Set context service instance to operate.

**USAGE**

   `ibmcloud cis instance-set [INSTANCE_NAME] [--unset]`

**ARGUMENTS**

   `INSTANCE_NAME` is the name of CIS service instance. If it is presented, set the context instance to operate, if not, show the current context instance.

**OPTIONS**

   `--unset`  Unset context instance.


### Create CIS Service Instance
{: #create-cis-service-instance}

**NAME**

   `instance-create` - Create a CIS service instance.

**USAGE**

   `ibmcloud cis instance-create INSTANCE_NAME PLAN`

**ARGUMENTS**

   `INSTANCE_NAME` is the name of CIS service instance.

   `PLAN` is the name or id of a service plan.

**OPTIONS**

   `--output`  Specify output format, only JSON is supported now.


### Delete CIS Service Instance
{: #delete-cis-service-instance}

**NAME**

   `instance-delete` - Delete a CIS service instance.

**USAGE**

   `ibmcloud cis instance-delete INSTANCE` 

**ARGUMENTS**

   `INSTANCE` is the name or ID of a CIS service instance.

**OPTIONS**

   `--force`  Delete instance without prompting for confirmation.


### Update CIS Service Instance
{: #update-cis-service-instance}

**NAME**

   `instance-update` - Update a CIS service instance.

**USAGE**

   `ibmcloud cis instance-update INSTANCE [--name NAME] [--plan PLAN]` 

**ARGUMENTS**

   `INSTANCE` is the name or ID of a CIS service instance.

**OPTIONS**

   `--name`    CIS service instance name.

   `--plan`    The name or id of a service plan.

   `--output`  Specify output format, only JSON is supported now.


### Get CIS Service Instance
{: #get-cis-service-instance}

**NAME**

   `instance` - Show details of a CIS service instance.

**USAGE**

   `ibmcloud cis instance INSTANCE` 

**ARGUMENTS**

   `INSTANCE` is the name or ID of a CIS service instance.

**OPTIONS**

   `--output`  Specify output format, only JSON is supported now.


### List CIS Service Plans
{: #list-cis-service-plans}

**NAME**

   `plans` - List all CIS service plans.

**USAGE**

   `ibmcloud cis plans [--refresh] [--output FORMAT]` 

**OPTIONS**

   `--refresh`  Force refresh from catalog.
   
   
## Routing
{: #routing}

Manipulate routing by using the following `routing` commands.   

### Show Routing Setting
{: #show-routing}

**NAME**

  `routing` - Get details of Routing settings. (enterprise plan only)

**USAGE**

  `ibmcloud cis routing DNS_DOMAIN_ID (--smart-routing | --tiered-caching) [-i, --instance INSTANCE_NAME]`

**ARGUMENTS**

   `DNS_DOMAIN_ID` is the ID of DNS domain.
   
**OPTIONS**

   `--smart-routing`  One of Routing features.

   `--tiered-caching` One of Routing features.

   `-i, --instance INSTANCE_NAME`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.


### Update Routing Setting
{: #update-routing}

**NAME**

  `routing-update` - Update Routing setting. (enterprise plan only)

**USAGE**

  `ibmcloud cis routing-update DNS_DOMAIN_ID  (--smart-routing (on | off) | --tiered-caching (on | off)) [-i, --instance INSTANCE_NAME]`

**ARGUMENTS**

   `DNS_DOMAIN_ID` is the ID of DNS domain.
   
**OPTIONS**

   `--smart-routing value`       One of Routing features. Valid values: "on", "off".

   `--tiered-caching value`      One of Routing features. Valid values: "on", "off".
   
   `-i, --instance INSTANCE_NAME`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.


### Display routing analytics
{: #display-routing-analytics}

**NAME**

   `routing-analytics` - Get analytics of smart-routing latency. (enterprise plan only)

**USAGE**

   `ibmcloud cis routing-analytics DNS_DOMAIN_ID [--colos] [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**ARGUMENTS**

   `DNS_DOMAIN_ID` is the id of DNS domain.

**OPTIONS**

   `--colos`                      Analytics of smart-routing latency colos.
   
   `-i, --instance INSTANCE_NAME`   Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.

   `--output FORMAT`     Specify output format, only JSON is supported now.


## TLS
{: #tls}

Manipulate TLS by using the following `tls` commands. 

### Show TLS setting
{: #show-tls-setting}

**NAME**

  `tls-settings` - Get TLS settings for a given domain.

**USAGE**

 `ibmcloud cis tls-settings DNS_DOMAIN_ID [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**OPTIONS**

  `-i, --instance INSTANCE_NAME`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.

  `--output FORMAT`              Specify output format, only JSON is supported now.


### Update TLS Settings
{: #update-tls-settings}

**NAME**

  `tls-settings-update` - Update tls settings for a given DNS domain.

**USAGE**

  `ibmcloud cis tls-settings-update DNS_DOMAIN_ID [--mode MODE] [--universal (true|false)] [--tls-1-2-only (on|off)] [--tls-1-3 (on|off|zrt)] [-i, --instance INSTANCE_NAME][--output FORMAT]`

**OPTIONS**

   `--mode value` Specify whether visitors can browse your website over a secure connection, and when they do, how CIS will connect to your origin server.
                Valid values: `off`, `client-to-edge`, `end-to-end-flexible`, `end-to-end-ca-signed`, `https-only-origin-pull`.
                See the following documentation link for detailed [TLS mode description](/docs/infrastructure/cis/ssl-options.html#tls-options).
                    

   `--universal value` Specify whether universal ssl is enabled for you domain. Valid values are `true` and
                                       `false`.

   `--tls-1-2-only value`  Specify whether Crypto TLS 1.2 feature is enable for your domain. Enabling this
                                       feature prevents use of previous versions. Valid values are `on` and `off`.

   `--tls-1-3 value`  Specify whether Crypto TLS 1.3 feature is enabled for your domain. Valid values are `on` and
                               `off`.

   `--min-tls-version value`     Only accept HTTPS requests that use at least the TLS protocol version specified.  Valid values: `1.0`, `1.1`, `1.2`, `1.3`.

   `-i, --instance INSTANCE_NAME`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.

   `--output FORMAT`              Specify output format, only JSON is supported now.


### List Certificates
{: #list-cert}

**NAME**

  `certificates` - List all certificates for a given DNS domain, including shared, dedicated and custom certificates.

**USAGE**

   `ibmcloud cis certificates DNS_DOMAIN_ID [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**OPTIONS**

   `-i, --instance INSTANCE_NAME`  (Optional) Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.

   `--output FORMAT`  (Optional) Specify output format, only JSON is supported now.

### Show a certificate
{: #show-cert}

**NAME**

  `certificate` - Get details of a given shared, dedicated, custom certificate.

**USAGE**

 `ibmcloud cis certificate DNS_DOMAIN_ID (--cert-id CERT_ID | --universal) [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**OPTIONS**

   `-i, --instance INSTANCE_NAME ` (Optional) Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.

   `--cert-id value:` ID of dedicated or custom certificate.

   `--universal:` Show universal certificate details.

   `--output FORMAT`  (Optional) Specify output format, only JSON is supported now.


### Order dedicated certificate
{: #order-dedicated-cert}
**NAME**

  `certificate-order` - Order a certificate pack with an optional list of hostnames for a given DNS domain.

**USAGE**

  `ibmcloud cis certificate-order DNS_DOMAIN_ID [--hostnames host1 --hostnames host2 ...] [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**OPTIONS**

   `--hostnames HOSTNAME`  (Optional) valid host names for the certificate packs. Add up to 50 custom hostnames - May
affect price.

   `-i, --instance INSTANCE_NAME`  (Optional) Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.

   `--output FORMAT` (Optional) Specify output format, only JSON is supported now.

### Upload certificate
{: #upload-cert}

**NAME**

 `certificate-upload` - Upload a custom certificate for a DNS domain

**USAGE**

  `ibmcloud cis certificate-upload DNS_DOMAIN_ID [-i, --instance INSTANCE_NAME] [-s, --json-str JSON_STR] [-j, --json-file JSON_FILE] [--output FORMAT]`

**OPTIONS**

   `-i, --instance INSTANCE_NAME`  (Optional) Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.

   `-s, --json-str JSON_STR`  The JSON data used to upload a custom certificate.
   * The required fields in JSON data are `certificate`, `private_key`:
     * `certificate` SSL certificate or certificate and the intermediate(s) for the domain.
     * `private_key` Private key for the domain.
   * The optional fields are bundle_method:
     * `bundle_method` Bundle method, default value is `compatible`, valid values are: `compatible`, `modern` and `user-defined`.

   Sample JSON data:
   
                      {
                        "certificate": "xxx",
                        "private_key": "xxx",
                        "bundle_method": "compatible"
                      }
                      
   `-j, --json-file JSON_FILE`  (Optional) A file contains input JSON data.

   `--output FORMAT`  (Optional) Specify output format, only JSON is supported now.


### Update certificate
{: #update-cert}

**NAME**

 `update-certificate` Update a custom certificate for a DNS domain.

**USAGE**

  `ibmcloud cis certificate-update DNS_DOMAIN_ID CERT_ID [-i, --instance INSTANCE_NAME] [-s, --json-str JSON_STR] [-j, --json-file JSON_FILE] [--output FORMAT]`

**OPTIONS**

`-i, --instance INSTANCE_NAME`  (Optional) Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.
`-s, --json-str JSON_STR`  The JSON data used to update a custom certificate.
   * The required fields in JSON data are `certificate`, `private_key`:
     * `certificate` SSL certificate or certificate and the intermediate(s) for the domain.
     * `private_key` Private key for the domain.
   * The optional fields are bundle_method:
     * `bundle_method` Bundle method, default value is `compatible`, valid values are: `compatible`, `modern` and `user-defined`.

   Sample JSON data:
                      {
                        "certificate": "xxx",
                        "private_key": "xxx",
                        "bundle_method": "compatible"
                      }

`-j, --json-file JSON_FILE`  (Optional) A file contains input JSON data.

`--output FORMAT`  (Optional) Specify output format, only JSON is supported now.


### Change the priority of custom certificate
{: #change-priority-custom-cert}

**NAME**

  `certificate-priority-change` - Change custom certificates' priority for a given DNS domain.

**USAGE**

  `ibmcloud cis certificate-priority-change DNS_DOMAIN_ID [-i, --instance INSTANCE_NAME] [-s, --json-str JSON_STR] [-j, --json-file JSON_FILE] [--output FORMAT]`

**OPTIONS**

   `-i, --instance INSTANCE_NAME`  (Optional) Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.

   `-s, --json-str JSON_STR`  The JSON data used to change the custom certificates' priority.
     * The required fields in JSON data are `certificates`:
     * `certificates` An array of objects with the follow fields.
     * `id` Custom certificate identifier.
     * `priority` The order/priority in which the certificate is used in a request. Higher numbers are tried first.

   Sample JSON data:
                      {
                        "certificates":[
                          {
                            "id":"5a7805061c76ada191ed06f989cc3dac",
                            "priority":2
                          },
                          {
                            "id":"da534493b38266b17fea74f3312be21c",
                          "priority":1
                          }
                        ]
                      }

   `-j, --json-file JSON_FILE`  (Optional) A file contains input JSON data.

   `--output FORMAT`  (Optional) Specify output format, only JSON is supported now.

### Delete certificate
{: #delete-cert}

**NAME**

  `certificate-delete` - Delete a dedicated or custom certificate.

**USAGE**

  `ibmcloud cis certificate-delete DNS_DOMAIN_ID CERT_ID [-i, --instance INSTANCE_NAME]`

**OPTIONS**

   `-i, --instance INSTANCE_NAME`  (Optional) Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.


###  List origin certificates
{: #list-origin-cert}

**NAME**

  `origin-certificates` - List all origin certificates for a given DNS domain.

**USAGE**

  `ibmcloud cis origin-certificates DNS_DOMAIN_ID [--instance INSTANCE_NAME] [--output FORMAT]`

**ARGUMENTS**

  `DNS_DOMAIN_ID`  The id of DNS domain.

**OPTIONS**
   `-i, --instance INSTANCE_NAME`  Instance name. If not set, the context instance specified by 'ibmcloud cis instance-set' is used.
   
   `--output FORMAT`    Specify output format, only JSON is supported now.


###  Create origin certificate
{: #create-origin-cert}

**NAME**

  `origin-certificate-create` - Create a CIS-signed certificate.

**USAGE**

  `ibmcloud cis origin-certificate-create DNS_DOMAIN_ID [--request-type REQUEST_TYPE] [--hostnames HOST_NAME1] [--hostnames HOST_NAME2] [--requested-validity DAYS] [--csr CSR] [--json-str JSON_STR | --json-file JSON_FILE] [--instance INSTANCE_NAME] [--output FORMAT]`

**ARGUMENTS**

  `DNS_DOMAIN_ID`  The id of DNS domain.

**OPTIONS**
   `--request-type REQUEST_TYPE`        Signature type desired on certificate. Valid values: "origin-rsa", "origin-ecc".

   `--hostnames HOSTNAME`           hostname or wildcard name bound to the certificate.
   
   `--requested-validity DAYS`  The number of days for which the certificate should be valid. (default: 5475)
   
   `--csr CSR`                 The Certificate Signing Request (CSR). If not set, CIS will generate one.
   
   `-s, --json-str JSON_STR`  The JSON data describing an origin certificate.

                   The required fields in JSON data are "request_type", "hostnames".
                       
                       "request_type": Signature type desired on certificate. Valid values: "origin-rsa", "origin-ecc".
                       "hostnames": Array of hostnames or wildcard names bound to the certificate.

                   The optional fields are "requested_validity", "csr".
                       
                       "requested_validity": The number of days for which the certificate should be valid. Valid values: "0", "7", "30", "90", "365", "730", "1095", "5475".
                       "csr": The Certificate Signing Request (CSR). If not set, CIS will generate one.

                   Sample JSON data:
                   
                   {
                       "request_type": "origin-rsa",
                       "hostnames": [
                           "*.example.com",
                           "example.com",
                     ],
                       "requested_validity": 5475,
                       "csr": "your_csr"
                   }
   `-j, --json-file JSON_FILE`  A file contains input JSON data.


   `-i, --instance INSTANCE_NAME`  Instance name. If not set, the context instance specified by 'ibmcloud cis instance-set' is used.
   
   `--output FORMAT`    Specify output format, only JSON is supported now.


###  Show a origin certificate
{: #show-origin-cert}

**NAME**

  `origin-certificate` - Get details of a given origin certificate.

**USAGE**

  `ibmcloud cis origin-certificate DNS_DOMAIN_ID CERT_ID [--instance INSTANCE_NAME] [--output FORMAT]`

**ARGUMENTS**

  `DNS_DOMAIN_ID`  The id of DNS domain.

  `CERT_ID` The id of Origin Certificate.


**OPTIONS**
   `-i, --instance INSTANCE_NAME`  Instance name. If not set, the context instance specified by 'ibmcloud cis instance-set' is used.
   
   `--output FORMAT`    Specify output format, only JSON is supported now.

###  Delete origin certificate
{: #delete-origin-cert}

**NAME**

  `origin-certificate-delete` - Delete an origin certificate.

**USAGE**

  `ibmcloud cis  origin-certificate-delete DNS_DOMAIN_ID CERT_ID [--instance INSTANCE_NAME]`

**ARGUMENTS**

  `DNS_DOMAIN_ID`  The id of DNS domain.

  `CERT_ID` The id of Origin Certificate.


**OPTIONS**
   `-i, --instance INSTANCE_NAME`  Instance name. If not set, the context instance specified by 'ibmcloud cis instance-set' is used.

## Web Application Firewall (WAF)
{: #waf}

Manipulate Web Application Firewalls by using the following `waf` commands. 

### Show WAF setting
{: #show-waf-setting}

**NAME**

  `waf-setting` - Show WAF setting.

**USAGE**

  `ibmcloud cis waf-setting DNS_DOMAIN_ID`
  
**ARGUMENTS**

   `DNS_DOMAIN_ID` is the ID of DNS domain.

**OPTIONS**

   `-i, --instance INSTANCE_NAME`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.



### Update WAF Setting
{: #update-waf-setting}

**NAME**

  `waf-setting-update` - Update WAF setting.

**USAGE**

  `ibmcloud cis waf-setting-update DNS_DOMAIN_ID WAF_MODE`

**ARGUMENTS**

   `DNS_DOMAIN_ID` is the ID of DNS domain.
   
   `WAF_MODE` is the mode of WAF setting. Valid values are:  `waf-enable` , `waf-disable`.

**OPTIONS**

   `-i, --instance INSTANCE_NAME`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.

### List WAF packages
{: #list-waf-packages}

**NAME**

  `waf-packages` - List all WAF packages.

**USAGE**

  `ibmcloud cis waf-packages DNS_DOMAIN_ID [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**ARGUMENTS**

   `DNS_DOMAIN_ID` is the ID of DNS domain.

**OPTIONS**

   `-i, --instance INSTANCE_NAME`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.
   
   `--output FORMAT`    Specify output format, only JSON is supported now.



### Show a WAF package
{: #show-waf-package}

**NAME**

  `waf-package` - Get detail of a package.

**USAGE**

  `ibmcloud cis waf-package DNS_DOMAIN_ID WAF_PACKAGE_ID [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**ARGUMENTS**

   `DNS_DOMAIN_ID` is the ID of DNS domain.
   
   `WAF_PACKAGE_ID` is the ID of WAF package.

**OPTIONS**

   `-i, --instance INSTANCE_NAME`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.

   `--output FORMAT`    Specify output format, only JSON is supported now.



### Update WAF OWASP package
{: #update-waf-owasp-package}

**NAME**

  `waf-package-set` - Update OWASP Package setting.

**USAGE**

  `ibmcloud cis waf-package-set DNS_DOMAIN_ID OWASP_PACKAGE_ID [--sensitivity  SENSITIVITY] [--action_mode MODE] [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**ARGUMENTS**

   `DNS_DOMAIN_ID` is the ID of DNS domain.
   
   `OWASP_PACKAGE_ID` is the package ID of OWASP.

**OPTIONS**

   `--sensitivity SENSITIVITY`   The sensitivity of the firewall package. Valid values: `high`, `medium`, `low`, `off`.
   
   `--action-mode MODE`   The default action that will be taken for rules under the firewall package. Valid values: `simulate`, `block`, `challenge`.

   `-i, --instance INSTANCE_NAME`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.

   `--output FORMAT`    Specify output format, only JSON is supported now.


### List WAF groups
{: #list-waf-groups}

**NAME**

  `waf-groups` - List WAF groups in a given WAF package.

**USAGE**

  `ibmcloud cis waf-groups DNS_DOMAIN_ID WAF_PACKAGE_ID [--page PAGE] [--per-page NUM] [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**ARGUMENTS**

   `DNS_DOMAIN_ID` is the ID of DNS domain.
   
   `WAF_PACKAGE_ID` is the ID of WAF package.

**OPTIONS**

   `--page PAGE`         Page number of paginated results. (default: 1)

   `--per-page NUM`    Number of groups per page.The min value is 5 and max value is 1000. (default: 50)

   `-i, --instance INSTANCE_NAME`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.

   `--output FORMAT`    Specify output format, only JSON is supported now.


### Show a WAF group
{: #show-waf-group}

**NAME**

  `waf-group` - Get detail of a WAF group.

**USAGE**

  `ibmcloud cis waf-group DNS_DOMAIN_ID WAF_PACKAGE_ID WAF_GROUP_ID [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**ARGUMENTS**

   `DNS_DOMAIN_ID` is the ID of DNS domain.
   
   `WAF_PACKAGE_ID` is the ID of WAF package.
   
   `WAF_GROUP_ID` is the ID of WAF group.
   
**OPTIONS**

   `-i, --instance INSTANCE_NAME`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.

   `--output FORMAT`    Specify output format, only JSON is supported now.


### Update a WAF group
{: #update-waf-group}

**NAME**

  `waf-group-mode-set` - Set mode of a WAF group.

**USAGE**

  `ibmcloud cis waf-group-mode-set DNS_DOMAIN_ID WAF_PACKAGE_ID WAF_GROUP_ID WAF_GROUP_MODE`

**ARGUMENTS**

   `DNS_DOMAIN_ID` is the ID of DNS domain.
   
   `WAF_PACKAGE_ID` is the ID of WAF package.
   
   `WAF_GROUP_ID` is the ID of WAF group.
   
   `WAF_GROUP_MODE` is the mode of WAF group. Valid values are: `on`, `off`.

**OPTIONS**

   `-i, --instance INSTANCE_NAME`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.

  `--output FORMAT`    Specify output format, only JSON is supported now.


### List WAF rules
{: #list-waf-rules}

**NAME**

  `waf-rules` - List all WAF rules of a given WAF package.

**USAGE**

  `ibmcloud cis waf-rules DNS_DOMAIN_ID WAF_PACKAGE_ID [--page PAGE] [--per-page NUM] [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**ARGUMENTS**

   `DNS_DOMAIN_ID` is the ID of DNS domain.
   
   `WAF_PACKAGE_ID` is the ID of WAF package.

**OPTIONS**

   `--page PAGE`                Page number of paginated results. (default: 1)

   `--per-page NUM`         Number of rules per page. (default: 50)

   `-i, --instance INSTANCE_NAME`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.
   
   `--output FORMAT`    Specify output format, only JSON is supported now.



### Show a WAF rule
{: #show-waf-rule}

**NAME**

  `waf-rule` - Get detail of a WAF rule.

**USAGE**

  `ibmcloud cis waf-rule DNS_DOMAIN_ID WAF_PACKAGE_ID WAF_RULE_ID [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**ARGUMENTS**

   `DNS_DOMAIN_ID` is the ID of DNS domain.
   
   `WAF_PACKAGE_ID` is the ID of WAF package.
   
   `WAF_RULE_ID` is the ID of WAF rule.

**OPTIONS**

   `-i, --instance INSTANCE_NAME`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.

   `--output FORMAT`    Specify output format, only JSON is supported now.


### Update a WAF rule
{: #update-waf-rule}

**NAME**

  `waf-rule-mode-set` - Set mode of a WAF rule.

**USAGE**

  `ibmcloud cis waf-rule-mode-set DNS_DOMAIN_ID WAF_PACKAGE_ID WAF_RULE_ID WAF_RULE_MODE [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**ARGUMENTS**

   `DNS_DOMAIN_ID` is the ID of DNS domain.
   
   `WAF_PACKAGE_ID` is the ID of WAF package.
   
   `WAF_RULE_ID` is the ID of WAF rule.
   
   `WAF_RULE_MODE` is the mode of WAF rule. Valid values are: `on`, `off`, `default`, `disable`, `simulate`, `block`,  `challenge`.

**OPTIONS**

   `-i, --instance INSTANCE_NAME`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.

   `--output FORMAT`    Specify output format, only JSON is supported now.


## Edge Functions
{: #edge-functions}

Manipulate how the Edge Functions performs using the following `edge-functions` commands:

### Create Edge Functions Script
{: #create-edge-functions-script}

**NAME**

  `edge-functions-script-create` - Create an edge functions script for a given service instance. (enterprise plan only)

**USAGE**

  `ibmcloud cis edge-functions-script-create SCRIPT_NAME (--script-str JAVASCRIPT_STR | --script-file JAVASCRIPT_FILE) [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**ARGUMENTS**

   * `SCRIPT_NAME`  is the name of an edge functions script.

**OPTIONS**

   * `--script-str value`          Script string. Eg: "addEventListener('fetch', event => { event.respondWith(fetch(event.request))})"

   * `--script-file value`         Script file.

   * `-i value, --instance value`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.

   * `--output value`              Specify output format, only JSON is supported now.


### Update Edge Functions Script
{: #update-edge-functions-script}

**NAME**

  `edge-functions-script-update` - Update an edge functions script for a given service instance. (enterprise plan only)

**USAGE**

  `ibmcloud cis edge-functions-script-update SCRIPT_NAME (--script-str JAVASCRIPT_STR | --script-file JAVASCRIPT_FILE) [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**ARGUMENTS**

   * `SCRIPT_NAME`  is the name of an edge functions script.

**OPTIONS**

   * `--script-str value`          Script string. Eg: "addEventListener('fetch', event => { event.respondWith(fetch(event.request))})"

   * `--script-file value`         Script file.

   * `-i value, --instance value`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.

   * `--output value`              Specify output format, only JSON is supported now.


### Delete Edge Functions Script
{: #delete-edge-functions-script}

**NAME**

  `edge-functions-script-delete` - Delete an edge functions script for a given service instance. (enterprise plan only)

**USAGE**

  `ibmcloud cis edge-functions-script-delete SCRIPT_NAME [-i, --instance INSTANCE_NAME]`

**ARGUMENTS**

   * `SCRIPT_NAME` is the name of an edge functions script.

**OPTIONS**

   * `-i value, --instance value`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.


### Show Edge Functions Script
{: #show-edge-functions-script}

**NAME**

  `edge-functions-script` - Display an edge functions script of a given service instance. (enterprise plan only)

**USAGE**

  `ibmcloud cis edge-functions-script SCRIPT_NAME [-i, --instance INSTANCE_NAME]`

**ARGUMENTS**

   * `SCRIPT_NAME` is the name of an edge functions script.

**OPTIONS**

   * `-i value, --instance value`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.


### List Edge Functions Scripts
{: #list-edge-functions-script}

**NAME**

  `edge-functions-scripts` - List all edge functions scripts of a given service instance. (enterprise plan only)

**USAGE**

  `ibmcloud cis edge-functions-scripts [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**OPTIONS**

   * `-i value, --instance value`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.

   * `--output value`              Specify output format, only JSON is supported now.


### Create Edge Functions Route
{: #create-edge-functions-route}

**NAME**

  `edge-functions-route-create` - Create an edge functions route for a given domain. (enterprise plan only)

**USAGE**

  `ibmcloud cis edge-functions-route-create DNS_DOMAIN_ID PATTERN_URL SCRIPT_NAME [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**ARGUMENTS**

   * `DNS_DOMAIN_ID` is the ID of DNS domain.

   * `PATTERN_URL` is the request URL which triggers the script.

   * `SCRIPT_NAME` is the name of an edge functions script.

**OPTIONS**

   * `-i value, --instance value`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.

   * `--output value`              Specify output format, only JSON is supported now.


### Update Edge Functions Route
{: #update-edge-functions-route}

**NAME**

  `edge-functions-route-update` - Update an edge functions route for a given domain. (enterprise plan only)

**USAGE**

  `ibmcloud cis edge-functions-route-update DNS_DOMAIN_ID ROUTE_ID PATTERN_URL SCRIPT_NAME [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**ARGUMENTS**

   * `DNS_DOMAIN_ID` is the ID of DNS domain.

   * `ROUTE_ID` is the ID of an edge functions route.

   * `PATTERN_URL` is the request URL which triggers the script.

   * `SCRIPT_NAME` is the name of an edge functions script.

**OPTIONS**

   * `-i value, --instance value`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.

   * `--output value`              Specify output format, only JSON is supported now.


### Delete Edge Functions Route
{: #delete-edge-functions-route}

**NAME**

  `edge-functions-route-delete` - Delete an edge functions route for a given domain. (enterprise plan only)

**USAGE**

  `ibmcloud cis edge-functions-route-delete DNS_DOMAIN_ID ROUTE_ID [-i, --instance INSTANCE_NAME]`

**ARGUMENTS**

   * `DNS_DOMAIN_ID` is the ID of DNS domain.

   * `ROUTE_ID` is the ID of an edge functions route.

**OPTIONS**

   * `-i value, --instance value`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.


### Show Edge Functions Route
{: #show-edge-functions-route}

**NAME**

  `edge-functions-route` - Display an edge functions route of a given domain. (enterprise plan only)

**USAGE**

  `ibmcloud cis edge-functions-route DNS_DOMAIN_ID ROUTE_ID [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**ARGUMENTS**

   * `DNS_DOMAIN_ID` is the ID of DNS domain.
   
   * `ROUTE_ID` is the ID of an edge functions route.

**OPTIONS**

   * `-i value, --instance value`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.
   * `--output value`              Specify output format, only JSON is supported now.


### List Edge Functions Routes
{: #list-edge-functions-route}

**NAME**

  `edge-functions-routes` - List all edge functions routes of a given domain. (enterprise plan only)

**USAGE**

  `ibmcloud cis edge-functions-routes DNS_DOMAIN_ID [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**ARGUMENTS**

   * `DNS_DOMAIN_ID` is the ID of DNS domain.

**OPTIONS**

   * `-i value, --instance value`  Instance name. If not set, the context instance specified by `ibmcloud cis instance-set` is used.

   * `--output value`              Specify output format, only JSON is supported now.


## Range App
{: #range-app}

Manipulate how the Range App performs using the following `range-app` commands:

### Create Range App
{: #create-range-app}

**NAME**

   `range-app-create` - Create a new range application. (enterprise plan only)

**USAGE**

   `ibmcloud cis range-app-create DNS_DOMAIN_ID --name NAME --edge-port EDGE_PORT --origin-direct ORIGIN_DIRECT [--origin-direct ORIGIN_DIRECT] [--proxy-protocol on|off] [--ip-firewall on|off]`
   
   `ibmcloud cis range-app-create DNS_DOMAIN_ID --name NAME --edge-port EDGE_PORT --origin-lb-name ORIGIN_LB_NAME --origin-lb-port ORIGIN_LB_PORT [--proxy-protocol on|off] [--ip-firewall on|off]`
   
   `ibmcloud cis range-app-create DNS_DOMAIN_ID -s JSON_STR`
   
   `ibmcloud cis range-app-create DNS_DOMAIN_ID -j JSON_FILE`

**ARGUMENTS**

   * `DNS_DOMAIN_ID` is the ID of DNS domain.

**OPTIONS**

   * `--name value`                 The name of DNS record for the range application.

   * `--edge-port value`            Port configuration at CIS's edge. (default: 22)

   * `--origin-direct value`        Destination addresses to the origin.

   * `--origin-lb-name value`       The Load Balancer name associated with the range application.

   * `--origin-lb-port value`       The Load Balancer port associated with the range application. (default: 22)

   * `--proxy-protocol value`       Control whether or not enable Proxy Protocol v1 to the origin. Valid values: "on", "off". (default: "off")
   
   * `--ip-firewall value`          Control whether or not enables the IP Firewall for this application. Valid values: "on", "off". (default: "off")

   * `-s value, --json-str value`   The JSON data describing a range application.
                                
      * The required fields in JSON data are "protocol", "dns".
                                    
         * "protocol": Port configuration at CIS's edge.
         * "dns": The name and type of DNS record for the range application.
            * "name": The name of DNS record for the range application.
            * "type": The type of DNS record associated with the application. Valid values: "CNAME".
                                
      * The optional fields are "origin_direct", "origin_dns", "origin_port", "proxy_protocol", "ip_firewall".
                                    
         * "origin_direct": A list of destination addresses to the origin.
         * "origin_dns": Method and parameters used to discover the origin server address via DNS.
         * "name": DNS record name.
         * "origin_port": The destination port at the origin.
         * "proxy_protocol": Control whether or not enable Proxy Protocol v1 to the origin. Valid values: "on", "off".
         * "ip_firewall": Control whether or not enables the IP Firewall for this application. Valid values: "on", "off".
                                
      Sample JSON data:
                                
          {
              "protocol": "tcp/22",
              "dns": {
                  "type": "CNAME",
                  "name": "ssh.example.com"
              },
              "origin_direct": [
                  "tcp://1.2.3.4:22",
                  "tcp://1.2.3.4:23",
                  "tcp://1.2.3.4:24"
              ],
              "proxy_protocol": false,
              "ip_firewall": false
          }

          {
              "protocol": "tcp/22",
              "dns": {
                  "type": "CNAME",
                  "name": "glb.example.com"
              },
              "origin_dns": {
                  "name": "name-to-glb.example.com"
              },
              "origin_port": 22,
              "proxy_protocol": false,
              "ip_firewall": false
          }

   * `-j value, --json-file value`  A file contains input JSON data.

   * `-i value, --instance value`   Instance name. If not set, the context instance specified by 'ibmcloud cis instance-set INSTANCE' is used.

   * `--output value`               Specify output format, only JSON is supported now.


### Update Range App
{: #update-range-app}

**NAME**

   `range-app-update` - Update an previously existing application's configuration. (enterprise plan only)

**USAGE**

   `ibmcloud cis range-app-update DNS_DOMAIN_ID APP_ID --origin-direct ORIGIN_DIRECT`

   `ibmcloud cis range-app-update DNS_DOMAIN_ID APP_ID [--add-origin-direct ORIGIN_DIRECT] [--remove-origin-direct ORIGIN_DIRECT]`

   `ibmcloud cis range-app-update DNS_DOMAIN_ID APP_ID [--origin-lb-name ORIGIN_LB_NAME] [--origin-lb-port ORIGIN_LB_PORT]`

   `ibmcloud cis range-app-update DNS_DOMAIN_ID APP_ID -s JSON_STR`

   `ibmcloud cis range-app-update DNS_DOMAIN_ID APP_ID -j JSON_FILE`

**ARGUMENTS**

   * `DNS_DOMAIN_ID` is the ID of DNS domain.
   * `APP_ID` is the ID of range application.

**OPTIONS**

   * `--name value`                 The name of DNS record for the range application.

   * `--edge-port value`            Port configuration at CIS's edge. (default: 22)

   * `--origin-direct value`        Destination addresses to the origin.

   * `--origin-lb-name value`       The Load Balancer name associated with the range application.

   * `--origin-lb-port value`       The Load Balancer port associated with the range application. (default: 22)

   * `--proxy-protocol value`       Control whether or not enable Proxy Protocol v1 to the origin. Valid values: "on", "off". (default: "off")
   
   * `--ip-firewall value`          Control whether or not enables the IP Firewall for this application. Valid values: "on", "off". (default: "off")

   * `-s value, --json-str value`   The JSON data describing a range application.
                                
      * The required fields in JSON data are "protocol", "dns".
                                    
         * "protocol": Port configuration at CIS's edge.
         * "dns": The name and type of DNS record for the range application.
            * "name": The name of DNS record for the range application.
            * "type": The type of DNS record associated with the application. Valid values: "CNAME".
                                
      * The optional fields are "origin_direct", "origin_dns", "origin_port", "proxy_protocol", "ip_firewall".
                                    
         * "origin_direct": A list of destination addresses to the origin.
         * "origin_dns": Method and parameters used to discover the origin server address via DNS.
         * "name": DNS record name.
         * "origin_port": The destination port at the origin.
         * "proxy_protocol": Control whether or not enable Proxy Protocol v1 to the origin. Valid values: "on", "off".
         * "ip_firewall": Control whether or not enables the IP Firewall for this application. Valid values: "on", "off".
                                
      Sample JSON data:
                                
          {
              "protocol": "tcp/22",
              "dns": {
                  "type": "CNAME",
                  "name": "ssh.example.com"
              },
              "origin_direct": [
                  "tcp://1.2.3.4:22",
                  "tcp://1.2.3.4:23",
                  "tcp://1.2.3.4:24"
              ],
              "proxy_protocol": false,
              "ip_firewall": false
          }

          {
              "protocol": "tcp/22",
              "dns": {
                  "type": "CNAME",
                  "name": "glb.example.com"
              },
              "origin_dns": {
                  "name": "name-to-glb.example.com"
              },
              "origin_port": 22,
              "proxy_protocol": false,
              "ip_firewall": false
          }

   * `-j value, --json-file value`  A file contains input JSON data.

   * `-i value, --instance value`   Instance name. If not set, the context instance specified by 'ibmcloud cis instance-set INSTANCE' is used.

   * `--output value`               Specify output format, only JSON is supported now.


### Delete Range App
{: #delete-range-app}

**NAME**

   `range-app-delete` - Delete a previously existing application. (enterprise plan only)

**USAGE**

   `ibmcloud cis range-app-delete DNS_DOMAIN_ID APP_ID [--instance INSTANCE]`

**ARGUMENTS**

   * `DNS_DOMAIN_ID` is the ID of DNS domain.

   * `APP_ID` is the ID of range application.

**OPTIONS**

   * `-i value, --instance value`  Instance name. If not set, the context instance specified by 'ibmcloud cis instance-set INSTANCE' will be used.


### Show Range App
{: #show-range-app}

**NAME**

   `range-app` - Get the application configuration of a specific application. (enterprise plan only)

**USAGE**

   `ibmcloud cis range-app DNS_DOMAIN_ID APP_ID [--instance INSTANCE] [--output FORMAT]`

**ARGUMENTS**

   * `DNS_DOMAIN_ID` is the ID of DNS domain.

   * `APP_ID` is the ID of range application.

**OPTIONS**

   * `-i value, --instance value`  Instance name. If not set, the context instance specified by 'ibmcloud cis instance-set INSTANCE' will be used.

   * `--output value`              Specify output format, only JSON is supported now.


### List Range Apps
{: #list-range-app}

**NAME**

   `range-apps` - Retrieve a list of currently existing range applications for a given DNS domain. (enterprise plan only)

**USAGE**

   `ibmcloud cis range-apps DNS_DOMAIN_ID [--instance INSTANCE] [--output FORMAT]`

**ARGUMENTS**

   * `DNS_DOMAIN_ID` is the ID of DNS domain.

**OPTIONS**

   * `-i value, --instance value`  Instance name. If not set, the context instance specified by 'ibmcloud cis instance-set INSTANCE' is used.

   * `--output value`              Specify output format, only JSON is supported now.


### Analytics Range App
{: #analytics-range-app}

**NAME**

   `range-analytics` - Analytics data for range applications. (enterprise plan only)

**USAGE**

   `ibmcloud cis range-analytics DNS_DOMAIN_ID [--metrics METRICS] [--dimensions DIMENSION] [--filters FILTERS] [--sort SORT] [--since SINCE] [--until UNTIL]`

   `ibmcloud cis range-analytics DNS_DOMAIN_ID --bytime [--time_delta DELTA] [--metrics METRICS] [--dimensions DIMENSION] [--filters FILTERS] [--sort SORT] [--since SINCE] [--until UNTIL]`

**ARGUMENTS**

   * `DNS_DOMAIN_ID` is the ID of DNS domain.

**OPTIONS**

   * `--metrics value`             One or more metrics to compute.
                               To get all metrics, set metrics to 'count,bytesIngress,bytesEgress,durationAvg,durationMedian,duration90th,duration99th'.

   * `--dimensions value`          Can be used to break down the data by given attributes.
                               To get all dimensions, set dimensions to 'event,appID,coloName,ipVersion'.

   * `--filters value`             Used to filter rows by one or more dimensions.
                               Filters can be combined using OR and AND boolean logic. AND takes precedence over OR in all the expressions.
                               The OR operator is defined using a comma (,) or OR keyword surrounded by whitespace.
                               The AND operator is defined using a semicolon (;) or AND keyword surrounded by whitespace.
                               Comparison options are: ==, !=, >, <, >=, <=.
                               An example value for filters is: 'event==connect AND coloName!=SFO'.

   * `--sort value`                The sort order for the result set. Sort fields must be included in metrics or dimensions.
                               An example value for sort is: '+count,-bytesIngress'.

   * `--since value`               Start of time interval to query, defaults to until - 6 hours.
                               This should be an absolute timestamp that conforms to RFC 3339.

   * `--until value`               End of time interval to query, defaults to current time.
                               This should be an absolute timestamp that conforms to RFC 3339.

   * `--bytime`                    Analytics data for range applications grouped by time interval.

   * `--time-delta value`          Used to select time series resolution. Valid values: "year", "quarter", "month", "week", "day", "hour", "dekaminute", "minute".
                               Only valid when '--bytime' is given.

   * `-i value, --instance value`  Instance name. If not set, the context instance specified by 'ibmcloud cis instance-set INSTANCE' will be used.

   * `--output value`              Specify output format, only JSON is supported now.
   

## Firewall Rule
{: #firewall-rule}

Manipulate how the Firewall Rule performs using the following `firewall-rule` commands:

### Create Firewall Rule
{: #create-firewall-rule}

**NAME**

   `firewall-rule-create` - Create a firewall-rule for a given DNS domain.

**USAGE**

   `ibmcloud cis firewall-rule-create DNS_DOMAIN_ID --expression EXPRESSION --action ACTION [--priority PRIORITY] [--paused on|off] [--description DESCRIPTION] [-i, --instance INSTANCE_NAME] [--output FORMAT]`

   `ibmcloud cis firewall-rule-create DNS_DOMAIN_ID (-s, --json-str JSON_STR | -j, --json-file JSON_FILE) [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**ARGUMENTS**

   * `DNS_DOMAIN_ID` is the ID of DNS domain.

**OPTIONS**

   * `--expression value`           A filter expression. E.g. "ip.src eq 93.184.216.0"
   * `--action value`               The rule action to perform. Valid values: "log", "allow", "challenge", "js_challenge", "block".
   * `--priority value`             The rule's priority. Valid values: 0 ~ 2147483647. Value "0" means to set to the default value.
   * `--description value`          To briefly describe the rule.
   * `--paused value`               Indicates if the rule is active. Valid values: "on", "off".Default value is "off".
   * `-s value, --json-str value`   The JSON data describing a firewall-rule.
                                
      * The required fields in JSON data are "expression", "action".
                                    
         * "expression": A filter expression. E.g. "ip.src eq 93.184.216.0"
         * "action": The rule action to perform. Valid values: "log", "allow", "challenge", "js_challenge", "block".
                                
      * The optional fields are "description", "priority", "paused".
                                    
         * "description": To briefly describe the rule.
         * "priority": The rule's priority. Valid values: 0 ~ 2147483647. Value "0" means to set to the default value.
         * "paused": Indicates if the rule is active. Valid values: "on", "off".Default value is "off".
                                
      Sample JSON data:
                                
          {
              "expression": "ip.src eq 93.184.216.1 and http.request.uri.path ~ \"^.*/wp-login.php$\"",
              "action": "allow",
              "priority": 100,
              "paused": false,
              "description": "do not challenge login from office"
          }
            
   * `-j value, --json-file value`  A file contains input JSON data.

   * `-i value, --instance value`   Instance name. If not set, the context instance specified by 'ibmcloud cis instance-set INSTANCE' is used.

   * `--output value`               Specify output format, only JSON is supported now.


### Update Firewall Rule
{: #update-firewall-rule}

**NAME**

   `firewall-rule-update` - Update a specific firewall-rule for a given DNS domain.

**USAGE**

   `ibmcloud cis firewall-rule-update DNS_DOMAIN_ID FIREWALL_RULE_ID [--expression EXPRESSION] [--action ACTION] [--priority PRIORITY] [--paused on|off] [--description DESCRIPTION] [-i, --instance INSTANCE_NAME] [--output FORMAT]`

   `ibmcloud cis firewall-rule-update DNS_DOMAIN_ID FIREWALL_RULE_ID (-s, --json-str JSON_STR | -j, --json-file JSON_FILE) [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**ARGUMENTS**

   * `DNS_DOMAIN_ID` is the ID of DNS domain.

   * `FIREWALL_RULE_ID` is the ID of firewall-rule.

**OPTIONS**

   * `--expression value`           A filter expression. E.g. "ip.src eq 93.184.216.0"
   
   * `--action value`               The rule action to perform. Valid values: "log", "allow", "challenge", "js_challenge", "block".

   * `--priority value`             The rule's priority. Valid values: 0 ~ 2147483647. Value "0" means to set to the default value.

   * `--description value`          To briefly describe the rule.

   * `--paused value`               Indicates if the rule is active. Valid values: "on", "off".Default value is "off".

   * `-s value, --json-str value`   The JSON data describing a firewall-rule.
                                Note: Fields "description", "priority", "paused" which aren't explicitly set in JSON data will be overwrited by the default value.
                                
      * The required fields in JSON data are "action".
                                    
         * "action": The rule action to perform. Valid values: "log", "allow", "challenge", "js_challenge", "block".
                                
      * The optional fields are "expression", "description", "priority", "paused".
                                    
         * "expression": A filter expression. E.g. "ip.src eq 93.184.216.0"
         * "description": To briefly describe the rule.
         * "priority": The rule's priority. Valid values: 0 ~ 2147483647. Value "0" means to set to the default value.
         * "paused": Indicates if the rule is active. Valid values: "on", "off".Default value is "off".
                                
      Sample JSON data:
                                
          {
              "expression": "ip.src eq 93.184.216.1 and http.request.uri.path ~ \"^.*/wp-login.php$\"",
              "action": "allow",
              "priority": 100,
              "paused": false,
              "description": "do not challenge login from office"
          }

   * `-j value, --json-file value`  A file contains input JSON data.

   * `-i value, --instance value`   Instance name. If not set, the context instance specified by 'ibmcloud cis instance-set INSTANCE' is used.

   * `--output value`               Specify output format, only JSON is supported now.


### Delete Firewall Rule
{: #delete-firewall-rule}

**NAME**

   `firewall-rule-delete` - Delete a specific firewall-rule for a given DNS domain.

**USAGE**

   `ibmcloud cis firewall-rule-delete DNS_DOMAIN_ID FIREWALL_RULE_ID [--instance INSTANCE]`

**ARGUMENTS**

   * `DNS_DOMAIN_ID` is the ID of DNS domain.

   * `FIREWALL_RULE_ID` is the ID of firewall-rule.

**OPTIONS**

   * `-i value, --instance value`  Instance name. If not set, the context instance specified by 'ibmcloud cis instance-set INSTANCE' is used.


### Show Firewall Rule
{: #show-firewall-rule}

**NAME**

   `firewall-rule` - Retrieve a specific firewall-rule for a given DNS domain.

**USAGE**

   `ibmcloud cis firewall-rule DNS_DOMAIN_ID FIREWALL_RULE_ID [--instance INSTANCE] [--output FORMAT]`

**ARGUMENTS**

   * `DNS_DOMAIN_ID` is the ID of DNS domain.

   * `FIREWALL_RULE_ID` is the ID of firewall-rule.

**OPTIONS**

   * `-i value, --instance value`  Instance name. If not set, the context instance specified by 'ibmcloud cis instance-set INSTANCE' will be used.

   * `--output value`              Specify output format, only JSON is supported now.


### List Firewall Rules
{: #list-firewall-rule}

**NAME**

   `firewall-rules` - Retrieve a list of currently existing firewall-rules for a given DNS domain.

**USAGE**

   `ibmcloud cis firewall-rules DNS_DOMAIN_ID [--instance INSTANCE] [--output FORMAT]`

**ARGUMENTS**

   * `DNS_DOMAIN_ID` is the ID of DNS domain.

**OPTIONS**

   * `-i value, --instance value`  Instance name. If not set, the context instance specified by 'ibmcloud cis instance-set INSTANCE' is used.

   * `--output value`              Specify output format, only JSON is supported now.


### Validate Firewall Rule
{: #validate-firewall-rule}

**NAME**

   `firewall-rule-validate` - Validate a firewall-rule expression.

**USAGE**

   `ibmcloud cis firewall-rule-validate DNS_DOMAIN_ID EXPRESSION [--instance INSTANCE] [--output FORMAT]`

**ARGUMENTS**

   * `DNS_DOMAIN_ID` is the ID of DNS domain.

   * `EXPRESSION` A filter expression. For example, "ip.src eq 93.184.216.0"

**OPTIONS**

   * `-i value, --instance value`  Instance name. If not set, the context instance specified by 'ibmcloud cis instance-set INSTANCE' is used.

   * `--output value`              Specify output format, only JSON is supported now.


## Custom Page
{: #custom-page}

Manipulate how the Custom Page performs using the following `custom-page` commands:

### Update Custom Page
{: #update-custom-page}

**NAME**

   `custom-page-update` - Update a specific custom page.

**USAGE**

   `ibmcloud cis custom-page-update PAGE_ID PAGE_URL [-d, --domain DNS_DOMAIN_ID] [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**ARGUMENTS**

   * `PAGE_ID` The name of the Custom Page type. Valid values: "basic_challenge", "country_challenge", "ip_block", "ratelimit_block", "serve_stale_content", "under_attack", "waf_block", "waf_challenge", "1000_errors", "500_errors".

   * `PAGE_URL` A URL that is associated with the Custom Page. E.g. "http://www.example.com/example.html". Value "default" means to use the default page.

**OPTIONS**

   * `-d value, --domain value`    DNS Domain ID.

   * `-i value, --instance value`  Instance name. If not set, the context instance specified by 'ibmcloud cis instance-set INSTANCE' is used.

   * `--output value`              Specify output format, only JSON is supported now.


### Show Custom Page
{: #show-custom-page}

**NAME**

   `custom-page` - Retrieve a specific custom page.

**USAGE**

   `ibmcloud cis custom-page PAGE_ID [-d, --domain DNS_DOMAIN_ID] [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**ARGUMENTS**

   * `PAGE_ID` The name of the Custom Page type. Valid values: "basic_challenge", "country_challenge", "ip_block", "ratelimit_block", "serve_stale_content", "under_attack", "waf_block", "waf_challenge", "1000_errors", "500_errors".

**OPTIONS**

   * `-d value, --domain value`    DNS Domain ID.

   * `-i value, --instance value`  Instance name. If not set, the context instance specified by 'ibmcloud cis instance-set INSTANCE' is used.

   * `--output value`              Specify output format, only JSON is supported now.


### List Custom Pages
{: #list-custom-page}

**NAME**

   `custom-pages` - Retrieve a list of currently existing custom pages.

**USAGE**

   `ibmcloud cis custom-pages [-d, --domain DNS_DOMAIN_ID] [-i, --instance INSTANCE_NAME] [--output FORMAT]`

**OPTIONS**

   * `-d value, --domain value`    DNS Domain ID.

   * `-i value, --instance value`  Instance name. If not set, the context instance specified by 'ibmcloud cis instance-set INSTANCE' is used.

   * `--output value`              Specify output format, only JSON is supported now.
