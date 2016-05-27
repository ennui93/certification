Object Storage and CDN - S3, Glacier, CloudFront
=================



S3
-----------

* Object Based, i.e. files not operating systems
* 0 byte to 5 Tb
    * For objects larger 100 mb consider Multi-part Upload API
* Unlimited storage
* Stored in Buckets
    * Up to 100 buckets by default per account
    * Universal namespace, i.e. bucket names must be unique
    * https://s3-eu-west-1.amazonaws.com/bucketname
* Consistency Model
    * Read-after write consistency for new objects
        * Can read immediately once stored in S3
    * Eventual consistency for overwrite PUTs and DELETEs
        * Can take time to propagate throughout S3
        * Immediate access can result in old or un-deleted objects
* Storage Classes or Tiers
    * S3-Standard
        * 99.99% availability
        * 99.999999999% durability
    * S3-IA - Infrequently Accessed
        * 99.9% availability
        * 99.999999999% durability
        * Cheaper
        * Just as durable and immediate access
        * Extra fee for accessing the objects
        * Can directly PUT into this Tier with STANDARD_IA in x-amz-storage-class header 
    * S3-RRS - Reduced Redundancy
        * 99.99% availability
        * Even cheaper
        * Not as durable, only 99.99%
        * Suitable only for reproducible objects, e.g. thumbnails
    * Glacier
        * The cheapest
        * 3-5 hours to read objects
* Fundamentals
    * Key
    * Value
    * Version ID
        * Versions
        * Delete markers
    * Metadata
    * Access Control Lists
* Version Control
    * Turn on once, suspend afterwards
    * Pay for each version
        * 10 version of 1 Gb file = pay for 10 Gb
    * Multi-Factor Authentication for deletes 
* Lifecycle Management
    * Separate for versioned and un-versioned storage
    * Change types or move to data based on rules
        * Transition to S3-IA 
            * 128 Kb and 30 days after creation
        * Archive to Glacier 
            * After 30 days in S3-IA
            * Or next day if not using S3-IA
        * Permanently delete
    * Cross-region Replication when Version Control is enabled
* Security
    * All new buckets and objects are Private
    * Bucket policies 
    * Object Access Control Lists
    * Can configure to write Access Logs
        * To same bucket
        * To other buckets
    * Encryption
        * In Transit
            * SSL/TLS
        * At Rest
            * Server Side
                * Managed Keys - SSE-S3
                * KMS - SSE-KMS
                    * Audit Trail
                    * Flexibility
                * Customer Provided Keys - SSE-C
            * Client Side Encryption
* Storage Gateway
    * Gateway Stored Volumes
        * Entire data set is stored on site and asynchronously backed up to S3
        * Two copies of data exist: site and s3
        * e.g. when unreliable internet connection
    * Gateway Cached Volumes
        * Entire data set is store on S3 and most frequently accessed data is cached on site
        * When internet connectivity is not a problem
    * Gateway Virtual Tape Libraries (VTL)
        * Used for backup with popular apps like NetBackup, Backup Exec, Veam etc
        * For replacing whole in-house backup environment
* Import/Export
    * Amazon Disk
        * To 
            * EBS
            * S3
            * Glacier
        * From
            * S3
    * Amazon Snowball
        * Only S3
        * Preferred

Cloud Front
-----------

* Edge Location
    * Where content is cached
    * Different from Regions/AZs
    * Not only READ, can also write (PUT) to Edge Locations
    * Objects are cached for the live of TTL
        * 24 h by default
    * Can clear cached objects
        * Extra fee for that
* Origin
    * Origin for files which CDN will distribute
        * S3 bucket
        * EC2 instance
        * ELB
        * Route53
* Distribution
    * Name given to CDN which groups Edge Locations
    * Web Distribution
    * RTMP Distribution


S3 Acceleration
-----------

* Enabled separately
* Not free, additional charge
* The further you are from S3 bucket the greater the transfer acceleration
* Works as
    * User Data -> Internet -> Closest Amazon Edge Node -> Amazon Backbone -> S3 Bucket
* As opposed to standard way
    * User Data -> Internet -> S3 bucket
