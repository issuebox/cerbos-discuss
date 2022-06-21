# ask for help
I have a slightly complex system and I don't know how to implement it, hope you can help me implement cerbos policies.
## description
* This is an article collaboration management system, assuming the project name is `demosite`. 
* In order to facilitate the management and collaboration of articles, I have added a two-level directory structure: `organization` and `channel`
    * Channels can only be created under an organization
    * Articles or article categories can only be created under the channel (of course, articles can be created under categories)
* The `demosite` project has three roles: `root`、`admin` and `member`. 
    * `root`、`admin` and `member` can create `organization`


### the tricky point is: organizations and channels also have their own roles
* There are three roles of every organization: `owner`、`admin`、`member`
    * `owner` is creator of the organization.
    * `owner` can let other users of `demosite` join to the organization and set their role to `admin` or `member`
    * The difference between `owner` and `admin` is: the `owner` can delete `admin` from the organization, but `admin` cannot delete `owner`
    * `owner` and `admin` can Create/Read/Update/Delete organization
    * `owner` and `admin` can `Create` channel and `Read` the organization's channels info.
    * the `member` can only read the organization and view the organization's channels info.
    * inheritance relationship is: member -> admin -> owner
    
* There are five roles of every channel: `owner`、`admin`、`editor`、`member`、`guest`
    *  `owner` is creator of the channel, which may be the organization's `owner` or `admin` role
    *  `owner` can let other users (of the organization parent) join to the channel and set their role to `admin`、`editor`、 `member` or `guest`
    *  The difference between `owner` and `admin` is: the `owner` can delete `admin` from the organization, but `admin` cannot delete `owner`
    *  `owner` and `admin` can Create/Read/Update/Delete channel
    *  `owner` and `admin` can Create/Read/Update/Delete articles/categories, or move articles to other categories
    *  the `editor` can `Update/Delete` his own articles and can `create` articles/categories
    *  the `editor` and `member` can `Create/Read` all the articles/categories
    *  the `guest` can only read public articles of the channel
    *  inheritance relationship is: guest -> member -> editor -> admin -> owner

## roles related database tables explain
### user table fields
* `id`
* `username`
* `password`
* `role`:  the role of `demosite`, value is `root`、 `admin`or `mumber`

### user-organization table fields
* `user_id`
* `org_id`
* `role`: the role of organization, value is `owner`、`admin` or `member`

### user-channel table fields
* `user_id`
* `org_id`
* `channel_id`
* `role`: the role of channel, value is `owner`、`admin`、`editor`、`member` or `guest`

## at last
I hope you can help me sort out how the permission system of this system should be designed. This is a kind of problem for me and has been bothering me for a long time. I've been learning cerbos for about a week and am familiar with the basic concepts and usage, but a system like this is a bit complicated for me. If possible, I hope you can help me achieve it completely.The relevant naming can be defined according to your own understanding and then implemented in the playground. I know my request is a bit excessive and I am willing to pay accordingly.




