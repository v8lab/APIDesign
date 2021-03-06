﻿# Global Restfull API

  | Change Version | API Version | Change notes | Change Date | Author |
  | - | - | - | - | - |
  | 1.0 | v3 |  | 2020-02-17 | Hardy, Michael|

# Summary

- Global
  - [site](#site)
  - [agent](#agent)
    - [permission](#permission)
    - [shift](#shift)
  - [role](#role)
    - [agent](#agent)
    - [permission](#permission)
  - [department](#department)
    - [agent](#agent)
    - [shift](#shift)
  - [contact](#contact)
    - [contact identity](#contact-identity)
  - [visitor](#visitor)
  - [public canned message category](#public-canned-message-category)
  - [public canned message](#public-canned-message)
  - [private canned message category](#private-canned-message-category)
  - [private canned message](#private-canned-message)
  - [agent away status](#agent-away-status)
  - [whitelisted login IP range](#whitelisted-login-ip-range)
  - [agent sso](#agent-sso)
  - [visitor sso](#visitor-sso)
  - [shift](#shift)
  - [audit log](#audit-log)


# Site
  You need the `Manage Site` permission to manage site

  + `GET /api/v3/globalSettings/site` - [Get profile of a site](#get-profile-of-a-site)
  + `PUT /api/v3/globalSettings/site` - [Update profile of a site](#update-profile-of-a-site)


## Site Related Object JSON format

### Site Object
  Each Comm100 account is treated as a Site and has a unique Site ID.

 | Name | Type | Include | Read-only | Mandatory | Default | Description |
 | - | - | :-: | :-: | :-: | :-: | - |
 |`id` | integer  | | yes | no |  |Site ID.|
 |`dateTimeFormat` | string| | no | no  | 'MM-dd-yyyy HH:mm:ss'|Date & Time Format of site, value options include : MM-dd-yyy HH:mm:ss, MM/dd/yyyy HH:mm:ss, dd-MM-yyyy HH:mm:ss, dd/MM/yyyy HH:mm:ss, yyyy-MM-dd HH:mm:ss, yyyy/MM/dd HH:mm:ss |
 |`timeZone` | string| | no | yes  |  | Time zone of site. value include all [Time Zone Option](#time-zone-options) Ids. |
 |`company` | string | | no | yes  | |Company name.|
 |`companySize` | string| | no | no  |  |The number of staff of the company, value options include: 1-20, 21-50, 51-100, 101-180, 181-310, 311-600, Above 600. |
 |`website` | string  | | no | yes  | |Company website. |
 |`registeredEmail` | string  | | yes | no  | |Email used for site registration.|
 |`phone` | string | | no | no  | |Company phone number.|
 |`fax` | string | | no | no  | |Company fax number.|
 |`mailingAddress` | string | | no | no  | |The mailing address of the company.|
 |`city` | string  | | no | no  | |City where the company located.|
 |`stateOrProvince` | string  | | no | no  | |State/Province where the company located.|
 |`countryOrRegion` | string | | no | no  | |Country/Region where the company located.|
 |`postalOrZipCode` | string | | no | no  | |Postal/Zip Code where the company located.|
 |`firstName` | string  | | no | yes  | |First Name of site registrant.|
 |`lastName` | string  | | no | yes  | |Last Name of site registrant.|

## Site Endpoints

### Get profile of a site
  `GET /api/v3/globalSettings/site`

#### Parameters
    No parameters
#### Response

  The response is [Site](#site-object) Object, just include base informations.

#### Example
Using curl
```
curl -H "Content-Type: application/json" -X GET https://api1.comm100.io/api/v3/globalSettings/site
```
Response
```json
HTTP/1.1 200 OK
Content-Type:  application/json
{
    "id": 10000,
    "dateTimeFormat":"yyyy-MM-dd hh:mm:ss",
    "timeZone":"canadaCentralStandardTime",
    "company":"BMW",
    "companySize": "Above 600",
    "website":"www.bmw.com",
    "registeredEmail":"bmw@gmail.com",
    "phone":"88654987",
    "fax":"58469215",
    "mailingAddress":"mail@bmw.com",
    "city":"Berlin",
    "stateOrProvince":"",
    "countryOrRegion":"Germany",
    "postalOrZipCode":"10115–14199",
    "firstName":"Jasn",
    "lastName":"Statham",
}
```


### Update profile of a site
  `PUT /api/v3/globalSettings/site`

#### Parameters

Request body

  The request body contains data with the [Site](#site-object) structure.

#### Response

  The response is the [Site](#site-object) Object.

#### Example
Using curl
```
curl -H "Content-Type: application/json" -d '{
    "dateTimeFormat"："yyyy-MM-dd hh:mm:ss"，
    "timeZone"："canadaCentralStandardTime"，
    "company"："BMW"，
    "companySize": "Above 600",
    "website"："www.bwm.com"，
    "registeredEmail"："bmw@gmail.com"，
    "phone"："88654987"，
    "fax"："58469215"，
    "mailingAddress"："mail@bmw.com"，
    "city"："Berlin"，
    "stateOrProvince"：""，
    "countryOrRegion"："Germany"，
    "postalOrZipCode"："10115–14199"，
    "firstName"："Jasn"，
    "lastName"："Statham"，
    ...,
}' -X PUT https://api1.comm100.io/api/v3/globalSettings/site
```
Response
```json 
HTTP/1.1 200 OK
Content-Type:  application/json
{
    "id": 10000,
    "dateTimeFormat": "yyyy-MM-dd hh:mm:ss",
    "timeZone": "canadaCentralStandardTime",
    "company": "BMW",
    "companySize": "Above 600",
    "website": "www.bwm.com",
    "registeredEmail": "bmw@gmail.com",
    "phone": "88654987",
    "fax": "58469215",
    "mailingAddress": "mail@bmw.com",
    "city": "Berlin",
    "stateOrProvince": "",
    "countryOrRegion": "Germany",
    "postalOrZipCode": "10115–14199",
    "firstName": "Jasn",
    "lastName": "Statham",
}
```


# Agent

You need the `Manage Agent & Agent Roles` permission to manage agents.

  + `GET /api/v3/globalSettings/agents` - [Get a list of agents in site](#get-a-list-of-agents-in-site)
  + `GET /api/v3/globalSettings/agents/{id}` - [Get an agent by id](#get-an-agent-by-id)
  + `GET /api/v3/globalSettings/roles/{roleId}/agents` - [Get a list of agents by role id](#get-a-list-of-agents-by-role-id)
  + `GET /api/v3/globalSettings/departments/{departmentId}/agents` - [Get a list of agents by department id](#get-a-list-of-agents-by-department-id)
  + `GET /api/v3/globalSettings/agents/me` - [Get current agent](#get-current-agent)
  + `POST /api/v3/globalSettings/agents` - [Create a new agent](#create-a-new-agent)
  + `POST /api/v3/globalSettings/agents/{id}:unlock` - [Unlock the agent](#unlock-the-agent)
  + `POST /api/v3/globalSettings/agents/{id}:changePassword` - [Admin set an agent's password](#admin-set-an-agents-password)
  + `POST /api/v3/globalSettings/agents/me:changePassword` - [Change own password](#change-own-password)
  + `PUT /api/v3/globalSettings/agents/{id}` - [Update agent info](#Update-agent-info)
  + `PUT /api/v3/globalSettings/agents/me` - [Update current agent info](#Update-current-agent-info)
  + `DELETE /api/v3/globalSettings/agents/{id}` - [Delete an agent](#delete-an-agent)

## Agent Related Object JSON format

### Agent Object
  This is the entity details for Agents.

  | Name | Type | Include | Read-only | Mandatory | Default | Description |
  | - | - | :-: | :-: | :-: | :-: | - |
  |`id` | integer  | | yes | no |  |  |
  |`email` | string| | yes | yes | | Agent login email address, can not be changed |
  |`displayName` | string  | | no | no | | Different Agents can have the same Display Name. If not offered, will set by first name.|
  |`firstName` | string  | | no | yes | | The first name of the agent|
  |`lastName` | string  | | no | yes | | The last name of agent|
  |`isAdmin` | bool| | no | no | false | Whether the agent is an administrator or not.|
  |`isActive` | bool| | no | no | true | Whether the agent is active or not.|
  |`phone` | string | | no | no | | Mobile phone number of the agent.|
  |`title` | string  | | no | no | | The title of the agent.|
  |`bio` | string  | | no | no | | The bio info of the agent.|
  |`timeZone` | string| | no | no |  | Time zone of the agent. Value includes all [Time Zone Option](#time-zone-options) Ids, if not offered, will use the site time zone.|
  |`datetimeFormat` | string| | no | no |  'MM-dd-yyyy HH:mm:ss' | Date & Time Format selected by agents to display on the site, Value options include : MM-dd-yyy HH:mm:ss, MM/dd/yyyy HH:mm:ss, dd-MM-yyyy HH:mm:ss, dd/MM/yyyy HH:mm:ss, yyyy-MM-dd HH:mm:ss, yyyy/MM/dd HH:mm:ss|
  |`createdTime` | DateTime | | no | no | UTC | The create time of the agent.|
  |`isLocked` | bool| | yes | no | false | Account will be locked after several failed login attempts.|
  |`lockedTime` | DateTime | | no | no | UTC | When the agent was locked.|
  |`lastLoginTime` | DateTime | | no | no | UTC | The time of the last login to Comm100 account (Control Panel or Agent Console).|
  |`permissionIds` | integer[]  |  | no | no | NULL | The list of permission ids.|
  |`permissions` | [Permission](#permission)[]  | yes| no | no | | Agent permission settings. |
  |`roleIds` | Guid[]  |  | no | no | NULL | The list of the role ids which the agent belongs to. If not offered, will use role id of "All Agents" as default. |
  |`roles` | [Role](#role)[]  |yes | no | no | | The list of the roles which the agent belongs to.|
  |`departmentIds` | Guid[]  |  | no | no | NULL | The list of the department ids which the agent belongs to.|
  |`departments` | [Department](#department)[]  |yes | no | no | | The list of the roles which the agent belongs to.|
  |`shiftIds` | Guid[]  |  | no | no | NULL | The list of the shift ids which the agent belongs to.|
  |`shifts` | [Shift](#shift)[]  | yes | no | no  | | The list of shifts which the agent belongs to.|



### Agent List Response Object

  Agent List Object for agent list Response, include count and page information.

  | Name | Type | Include | Read-only | Mandatory | Default | Description |
  | - | - | :-: | :-: | :-: | :-: | - |
  |`count` | integer  | no | yes | no | | The total count of queries |
  |`nextPage` | string  | no | yes | no | | The next page url of the query  |
  |`previousPage` | string  | no | yes | no | | The previous page url of the query  |
  |`agents`|   [Agent](#agent-object)[]| no | yes| no | | A list of agents. |



## Agent Endpoints

### Get a list of agents in site
  `GET/api/v3/globalSettings/agents`

#### Parameters
   Query string

  | Name  | Type | Required  | Default | Description |
  | - | - | - | - | - |
  |`include`|string|no||Available value:`department`,`role`,`permission`,`shift`  |
  |`keywords` | string | no  |  | Filter by keywords in agent display name, email address. |
  |`pageIndex`|integer|no| 1 | The page index of the query. |
  |`pageSize`|integer|no| 10 | The page size of the query. |


#### Response

  The response is an [Agent List Response ](#agent-list-response-object) Object.

#### Example
Using curl
```
curl -H "Content-Type: application/json" -X GET https://api1.comm100.io/api/v3/globalSettings/agents
```
Response
```json
HTTP/1.1 200 OK
Content-Type:  application/json
{
    "count": 1234,
    "nextPage": "https://api1.comm100.io/api/v3/globalSettings/agents?pageIndex=2",
    "previousPage": "",
    "agents": [{
        "id": 68,
        "email": "Tom@gmail.com",
        "displayName":"Tom",
        "firstName":"Tom",
        "lastName":"Green",
        "title":"CEO",
        ...
    },
    ...
    ]
}
```

### Get an agent by id
+ `GET /api/v3/globalSettings/agents/{id}`

#### Parameters

Path parameters

| Name  | Type | Required  | Description |
| - | - | - | - |
|`id` | integer | yes  |  the id of the agent |

Query string

 | Name  | Type | Required  | Default | Description |
 | - | - | - | - | - |
 |`include`|string|no||Available value:`department`,`role`,`permission`,`shift` |

#### Response

The response is an [Agent](#agent-object) Object.

#### Example
Using curl
```
curl -H "Content-Type: application/json" -X GET https://api1.comm100.io/api/v3/globalSettings/agents/68?include=role,department
```
Response
```json
HTTP/1.1 200 OK
Content-Type:  application/json
{
    "id": 68,
    "email": "Tom@gmail.com",
    "displayName":"Tom",
    "firstName":"Tom",
    "lastName":"Green",
    "title":"CEO",
    ...
}
```

### Get a list of agents by role id
  `GET /api/v3/globalSettings/roles/{roleId}/agents`

#### Parameters

Path parameters

  | Name  | Type | Required  | Description |
  | - | - | - | - |
  |`roleId` | Guid | yes  |  The id of the role |

   Query string

  | Name  | Type | Required  | Default | Description |
  | - | - | - | - | - |
  |`include`|string|no||Available value:`department`,`role`,`permission`,`shift` |
  |`pageIndex`|integer|no| 1 | The page index of the query. |
  |`pageSize`|integer|no| 10 | The page size of the query. |

#### Response

The response is an [Agent List Response](#agent-list-response-object) Object.

#### Example
Using curl
```
curl -H "Content-Type: application/json" -X GET https://api1.comm100.io/api/v3/globalSettings/roles/4487fc9d-92e6-4487-a2e8-92e68d6892e6/agents
```
Response
```json
HTTP/1.1 200 OK
Content-Type:  application/json
{
    "count": 1234,
    "nextPage": "https://api1.comm100.io/api/v3/globalSettings/roles/4487fc9d-92e6-4487-a2e8-92e68d6892e6/agents?pageIndex=2",
    "previousPage": "",
    "agents": [{
        "id": 68,
        "email": "Tom@gmail.com",
        "displayName":"Tom",
        "firstName":"Tom",
        "lastName":"Green",
        "title":"CEO",
        ...
    },
    ...
    ]
}
```


### Get a list of agents by department id
  `GET /api/v3/globalSettings/departments/{departmentId}/agents`

#### Parameters
   Path parameters

  | Name  | Type | Required  | Description |
  | - | - | - | - |
  |`departmentId` | Guid | yes  |  The id of the department |

   Query string

  | Name  | Type | Required  | Default | Description |
  | - | - | - | - | - |
  |`include`| string | no ||Available value:`department`,`role`,`permission`,`shift` |
  |`pageIndex`| integer | no | 1 |The page index of the query. |
  |`pageSize`| integer | no | 10 |The page size of the query. |


#### Response

The response is an [Agent List Response](#agent-list-response-object) Object.

#### Example
Using curl
```
curl -H "Content-Type: application/json" -X GET https://api1.comm100.io/api/v3/globalSettings/departments/42dwdaww-92e6-4487-a2e8-92e68d68a2e8/agents
```
Response
```json
HTTP/1.1 200 OK
Content-Type:  application/json
{
    "count": 1234,
    "nextPage": "https://api1.comm100.io/api/v3/globalSettings/departments/42dwdaww-92e6-4487-a2e8-92e68d68a2e8/agents?pageIndex=2",
    "previousPage": "",
    "agents": [{
        "id": 68,
        "email": "Tom@gmail.com",
        "displayName":"Tom",
        "firstName":"Tom",
        "lastName":"Green",
        "title":"CEO",
        ...
    },
    ...
    ]
}
```


### Get current agent
+ `GET /api/v3/globalSettings/agents/me`

#### Parameters
  Query string

  | Name  | Type | Required  | Default | Description |
  | - | - | - | - | - |
  |`include`|string|no||Available value:`department`,`role`,`permission`,`shift` |

#### Response

The response is an [Agent](#agent-object) Object.

#### Example
Using curl
```
curl -H "Content-Type: application/json"
-X GET https://api1.comm100.io/api/v3/globalSettings/agents/me?include=role,department
```
Response
```json
HTTP/1.1 200 OK
Content-Type:  application/json
{
    "id": 68,
    "email": "Tom@gmail.com",
    "displayName":"Tom",
    "firstName":"Tom",
    "lastName":"Green",
    "title":"CEO",
    ...
}
```

### Create a new agent
  `POST /api/v3/globalSettings/agents`

####  Parameters

Request body

  The request body contains data with the [Agent](#agent-object) structure.

example:
```json
{
    "email": "Tom@gmail.com",
    "displayName":"Tom",
    "firstName":"Tom",
    "lastName":"Green",
    "title":"CEO",
    "roleIds": [
      "4487fc9d-92e6-4487-a2e8-92e68d6892e6",
      ...,
    ]
    ...,
}
```

#### Response
The response is:
  [Agent](#agent-object) Object.

#### Example
Using curl
```
curl -H "Content-Type: application/json" -d '{
    "email": "Tom@gmail.com",
    "displayName":"Tom",
    "firstName":"Tom",
    "lastName":"Green",
    "title":"CEO",
    "roleIds": [
      "4487fc9d-92e6-4487-a2e8-92e68d6892e6",
      ...,
    ]
    ...,
} ' -X POST https://api1.comm100.io/api/v3/globalSettings/agents
```
Response
```json
HTTP/1.1 201 Created
Content-Type:  application/json
Location: https://api1.comm100.io/api/v3/globalSettings/agents/68
{
    "id": 68,
    "email": "Tom@gmail.com",
    "displayName":"Tom",
    "firstName":"Tom",
    "lastName":"Green",
    "title":"CEO",
    "roleIds": [
      "4487fc9d-92e6-4487-a2e8-92e68d6892e6",
      ...,
    ]
    ...,
}
```


### Unlock the agent
  `POST /api/v3/globalSettings/agents/{id}:unlock`

####  Parameters
Path parameters

  | Name  | Type | Required  | Description |
  | - | - | - | - |
  |`id` | integer | yes  |  The id of the agent |

#### Response
HTTP/1.1 204 No Content

#### Example
Using curl
```
curl -H "Content-Type: application/json" -d '{} ' -X POST https://api1.comm100.io/api/v3/globalSettings/agents/68:unlock
```
Response
```json
HTTP/1.1 204 No Content
```

### Admin set an agent's password
  `POST /api/v3/globalSettings/agents/{id}:changePassword`

####  Parameters
Path parameters

  | Name  | Type | Required  | Description |
  | - | - | - | - |
  |`id` | integer | yes  |  The id of the agent |

Request body

  | Name  | Type | Required | Default | Description |
  | - | - | - | - | - |
  | `password` | string | yes  |  | The new password of agent |

  example:
  ```json
  {
    "password": "Aa5847lkdsc&d",
  }
  ```

#### Response
HTTP/1.1 204 No Content

#### Example
Using curl
```
curl -H "Content-Type: application/json" -d '{
    "password": "Aa5847lkdsc&d",
}' -X POST https://api1.comm100.io/api/v3/globalSettings/agents/68:changePassword
```
Response
```json
HTTP/1.1 204 No Content
```

### Change own password
  `POST /api/v3/globalSettings/agents/me:changePassword`

####  Parameters

Request body

  | Name  | Type | Required | Default | Description |
  | - | - | - | - | - |
  | `currentPassword` | string | yes  |  | The current password of agent |
  | `newPassword` | string | yes  |  | The new password of agent |

  example:
  ```json
  {
    "currentPassword": "Aa2541554",
    "newPassword": "Aa5847lkdsc&d",
  }
  ```

#### Response
HTTP/1.1 204 No Content

#### Example
Using curl
```
curl -H "Content-Type: application/json" -d ' {
    "currentPassword": "Aa2541554",
    "newPassword": "Aa5847lkdsc&d",
  }' -X POST https://api1.comm100.io/api/v3/globalSettings/agents/me:changePassword
```

Response
```json
HTTP/1.1 204 No Content
```


### Update agent info
  `PUT /api/v3/globalSettings/agents/{id}`

####  Parameters
Path parameters

  | Name  | Type | Required  | Description |
  | - | - | - | - |
  |`id` | integer | yes  |  The id of the agent |

Request body

  The request body contains data with the [Agent](#agent-object) structure.

  example:
```json
{
    "id": 68,
    "email": "Tom@gmail.com",
    "displayName":"Tom",
    "firstName":"Tom",
    "lastName":"Green",
    "title":"CEO",
    "roleIds": [
      "4487fc9d-92e6-4487-a2e8-92e68d6892e6",
      ...,
    ]
    ...,
}
```

#### Response
The response is:
  [Agent](#agent-object) Object.

#### Example
Using curl
```
curl -H "Content-Type: application/json" -d '{
    "email": "Tom@gmail.com",
    "displayName":"Tom",
    "firstName":"Tom",
    "lastName":"Green",
    "title":"CEO",
    "roleIds": [
      "4487fc9d-92e6-4487-a2e8-92e68d6892e6",
      ...,
    ]
    ...,
} ' -X PUT https://api1.comm100.io/api/v3/globalSettings/agents/68
```
Response
```json
HTTP/1.1 200 OK
Content-Type: application/json
Location: https://api1.comm100.io/api/v3/globalSettings/agents/68
{
    "id": 68,
    "email": "Tom@gmail.com",
    "displayName":"Tom",
    "firstName":"Tom",
    "lastName":"Green",
    "title":"CEO",
    "roleIds": [
      "4487fc9d-92e6-4487-a2e8-92e68d6892e6",
      ...,
    ]
    ...,
}
```

### Update current agent info

  `PUT /api/v3/globalSettings/agents/me`

####  Parameters

Request body

  The request body contains data with the [Agent](#agent-object) structure.

example:
```json
{
    "id": 68,
    "email": "Tom@gmail.com",
    "displayName":"Tom",
    "firstName":"Tom",
    "lastName":"Green",
    "title":"CEO",
    "roleIds": [
      "4487fc9d-92e6-4487-a2e8-92e68d6892e6",
      ...,
    ]
    ...,
}
```

#### Response
The response is: 
  [Agent](#agent-object) Object.

#### Example
Using curl
```
curl -H "Content-Type: application/json" -d '{
    "id": 68,
    "email": "Tom@gmail.com",
    "displayName":"Tom",
    "firstName":"Tom",
    "lastName":"Green",
    "title":"CEO",
    "roleIds": [
      "4487fc9d-92e6-4487-a2e8-92e68d6892e6",
      ...,
    ]
    ...,
} ' -X PUT https://api1.comm100.io/api/v3/globalSettings/me
```

Response
```json
HTTP/1.1 200 OK
Content-Type: application/json
Location: https://api1.comm100.io/api/v3/globalSettings/agents/me
{
    "id": 68,
    "email": "Tom@gmail.com",
    "displayName":"Tom",
    "firstName":"Tom",
    "lastName":"Green",
    "title":"CEO",
    "roleIds": [
      "4487fc9d-92e6-4487-a2e8-92e68d6892e6",
      ...,
    ]
    ...,
}
```


### Delete an agent
  `DELETE /api/v3/globalSettings/agents/{id}`

#### Parameters
Path parameters

  | Name  | Type | Required  | Description |
  | - | - | - | - |
  |`id` | integer | yes  |  the agent id |

#### Response
HTTP/1.1 204 No Content

#### Example
Using curl
```
curl -X DELETE https://api1.comm100.io/api/v3/globalSettings/agents/68
```
Response
```json
HTTP/1.1 204 No Content
```



# Role
You need the `Manage Agent & Agent Roles` permission to manage roles.

  + `GET /api/v3/globalSettings/roles` - [Get a list of roles in site](#get-a-list-of-roles-in-site)
  + `GET /api/v3/globalSettings/roles/{id}` - [Get a role by id](#get-a-role-by-id)
  + `POST /api/v3/globalSettings/roles` - [Create a new role](#create-a-new-role)
  + `PUT /api/v3/globalSettings/roles/{id}` - [Update a role](#update-a-role)
  + `DELETE /api/v3/globalSettings/roles/{id}` - [Delete a role](#delete-a-role)

## Role Related Object JSON format

### Role Object
  | Name | Type | Include | Read-only | Mandatory | Default | Description |
  | - | - | :-: | :-: | :-: | :-: | - |
  |`id` | Guid| | yes | no | |  |
  |`name` | string| | no | yes | | Name.|
  |`description` | string| | no | no | | Description of this role.|
  |`type` | string | | yes | no | custom | Options: administrator, agent, custom; administrator and agent are the system role types. They cannot be deleted. |
  |`agentIds` | int[] | | no | no | NULL | The selected agents for this role. |
  |`agents` | [Agent](#agent)[] | yes | no | no | | The selected agents for this role.|
  |`permissionIds` | int[] | | no | no | NULL | The list of permission ids assigned to this role.|
  |`permissions` | [Permission](#permission)[] | yes | no | no | | Permissions assigned to this role.|


## Role Endpoints

### Get a list of roles in site
  `GET /api/v3/globalSettings/roles`

#### Parameters
  Query string

  | Name  | Type | Required  | Default | Description |
  | - | - | - | - | - |
  |`include`|string|no||Available value:`agent`,`permission` |

#### Response

The response is a list of [Role](#role) objects

#### Example
Using curl
```
curl -H "Content-Type: application/json" -X GET https://api1.comm100.io/api/v3/globalSettings/roles?include=permission
```
Response
```json
HTTP/1.1 200 OK
Content-Type:  application/json
[{
  "id": "4487fc9d-92e6-4487-a2e8-92e68d6892e6",
  "name": "markting",
  "description": "yyyy-MM-dd hh:mm:ss",
  "type": "custom",
  "agentIds":  [
    68,
    ...,
  ],
  "agents": [],
  "permissionIds" :
    [
      201,
      205,
      ...,
    ],
    "permissions": []
},
...,
]
```

### Get a role by id

  `GET /api/v3/globalSettings/roles/{id}`


#### Parameters
  Path parameters

  | Name  | Type | Required  | Description |
  | - | - | - | - |
  |`id` | Guid | yes  |  The id of the role |

  Query string

  | Name  | Type | Required  | Default | Description |
  | - | - | - | - | - |
  |`include`|string|no|| Available value:`agent`,`permission` |

#### Response

The response is a [Role](#role) object

#### Example
Using curl
```
curl -H "Content-Type: application/json" -X GET https://api1.comm100.io/api/v3/globalSettings/roles/4487fc9d-92e6-4487-a2e8-92e68d6892e6
```
Response
```json
HTTP/1.1 200 OK
Content-Type:  application/json
{
  "id": "4487fc9d-92e6-4487-a2e8-92e68d6892e6",
  "name": "markting",
  "description": "yyyy-MM-dd hh:mm:ss",
  "type": "custom",
  "agentIds":  [
    68,
    ...,
  ],
  "agents": [],
  "permissionIds" :
    [
      201,
      205,
      ...,
    ],
    "permissions": []
}
```


### Create a new role

  `POST /api/v3/globalSettings/roles`

####  Parameters

Request body

  The request body contains data with the [Role](#role-object) structure.

  example:
```json
 {
      "Name": "markting",
      "Description": "yyyy-MM-dd hh:mm:ss",
      "Type": "custom",
      "agentIds":  [
        68,
        ...,
      ],
      "agents": [],
      "permissionIds" :
        [
         201,
         205,
         ...,
        ],
      "permissions": []
    }
```

#### Response
The response is a [Role](#role-object) object

#### Example
Using curl
```
curl -H "Content-Type: application/json" -d ' {
      "id": "4487fc9d-92e6-4487-a2e8-92e68d6892e6",
      "Name": "markting",
      "Description": "yyyy-MM-dd hh:mm:ss",
      "Type": "custom",
      "agentIds":  [
        68,
        ...,
      ],
      "agents": [],
      "permissionIds" :
        [
         201,
         205,
         ...,
        ],
      "permissions": []
    }' -X POST https://api1.comm100.io/api/v3/globalSettings/roles
```
Response
```json
HTTP/1.1 201 Created
Content-Type:  application/json
Location: https://api1.comm100.io/api/v3/globalSettings/roles/4487fc9d-92e6-4487-a2e8-92e68d6892e6
 {
    "id": "4487fc9d-92e6-4487-a2e8-92e68d6892e6",
    "name": "markting",
    "description": "yyyy-MM-dd hh:mm:ss",
    "type": "custom",
    "agentIds":  [
      68,
      ...,
    ],
    "agents": [],
    "permissionIds" :
      [
        201,
        205,
        ...,
      ],
    "permissions": []
  }
```


### Update a role
  `PUT /api/v3/globalSettings/roles/{id}`

####  Parameters
Path parameters

  | Name  | Type | Required  | Description |
  | - | - | - | - |
  |`id` | Guid | yes  |  The id of the role |

Request body

  The request body contains data with the  [Role](#role-object) structure.

  example:
```json
 {
    "id": "4487fc9d-92e6-4487-a2e8-92e68d6892e6",
    "name": "markting",
    "description": "yyyy-MM-dd hh:mm:ss",
    "type": "custom",
    "agentIds":  [
      68,
      ...,
    ],
    "agents": [],
    "permissionIds" :
      [
        201,
        205,
        ...,
      ],
    "permissions": []
  }
```

#### Response
The response is a [Role](#role-object) object

#### Example
Using curl
```
curl -H "Content-Type: application/json" -d ' {
      "name": "markting",
      "description": "yyyy-MM-dd hh:mm:ss",
      "type": "custom",
      "agentIds":  [
        68,
        ...,
      ],
      "agents": [],
      "permissionIds" :
      [
        201,
        205,
        ...,
      ],
      "permissions": []
    } ' -X PUT https://api1.comm100.io/api/v3/globalSettings/roles/4487fc9d-92e6-4487-a2e8-92e68d6892e6
```
Response
```json
HTTP/1.1 201 Created
Content-Type:  application/json
Location: https://api1.comm100.io/api/v3/globalSettings/roles/4487fc9d-92e6-4487-a2e8-92e68d6892e6
{
  "id": "4487fc9d-92e6-4487-a2e8-92e68d6892e6",
  "name": "markting",
  "description": "yyyy-MM-dd hh:mm:ss",
  "type": "custom",
  "agentIds":  [
    68,
    ...,
  ],
  "agents": [],
  "permissionIds" :
    [
      201,
      205,
      ...,
    ],
  "permissions": []
}
```

### Delete a role
  `DELETE /api/v3/globalSettings/roles/{id}`

#### Parameters
Path parameters

  | Name  | Type | Required  | Description |
  | - | - | - | - |
  |`id` | Guid | yes  |  The role id |

#### Response
HTTP/1.1 204 No Content

#### Example
Using curl
```
curl -X DELETE https://api1.comm100.io/api/v3/globalSettings/roles/4487fc9d-92e6-4487-a2e8-92e68d6892e6
```
Response
```json
HTTP/1.1 204 No Content
```


# Department

You need the `Manage departments` permission to manage departments.

  + `GET /api/v3/globalSettings/departments` - [Get a list of departments in site](#get-all-departments)
  + `GET /api/v3/globalSettings/departments/{id}` - [Get a department by id](#get-a-department)
  + `POST /api/v3/globalSettings/departments` - [Create a new department](#create-a-new-department)
  + `PUT /api/v3/globalSettings/departments/{id}` - [Update a department](#update-a-department)
  + `DELETE /api/v3/globalSettings/departments/{id}` - [Delete a department](#delete-a-department)


## Department Related Object JSON format

### Department Object

  | Name | Type | Include | Read-only | Mandatory | Default | Description |
  | - | - | :-: | :-: | :-: | :-: | - |
  |`id` | Guid| | yes | no | |  |
  |`name` | string | | no | yes | |  |
  |`description` | string | | no | no | |  |
  |`isAvailableInChat` | bool| | no | no | false | When it is false, the Department will not be displayed in the Pre-chat window, Department drop down list, routing rules, chat transfer rules etc. Default: true.|
  |`isAvailableInTicketingAndMessaging` | bool| | no | no | false | When it is false, the department name will not be displayed in the 'Assigned Department' field. Default: true.|
  |`offlineMessageMailTo` | string | | no | no | All agents in the department | The value options: All agents in the department, The email address(es).  |
  |`offlineMessageEmailAddresses` | string  | | no | no | | Specific email addresses that offline message are send to. Available and required when Offline Message Mail Type is ‘The email address(es)’.|
  |`agentIds` | int[] | | no | no | NULL | The selected agents for this department.|
  |`agents` | [Agent](#agent)[]| yes | no | no |  |  |
  |`shiftIds` | Guid[]  |  | no | no | NULL | The list of the shift ids which the department belongs to.|
  |`shifts` | [Shift](#shift)[]| yes | no | no |  |  |

## Department Endpoints

### get all departments
  `GET /api/v3/globalSettings/departments`

#### Parameters
  Query string

  | Name  | Type | Required  | Default | Description |
  | - | - | - | - | - |
  |`include`|string|no||Available value:`agent`,`shift` |

#### Response

  The response is a list of [Department](#department-object) Object.

#### Example
Using curl
```
curl -H "Content-Type: application/json" -X GET https://api1.comm100.io/api/v3/globalSettings/departments
```
Response
```json
HTTP/1.1 200 OK
Content-Type:  application/json
[{
  "id": "bs22qa68-92e6-4487-a2e8-8234fc9d1f48",
  "name": "markting",
  "description": "markting departments",
  "isAvailableInChat": "yes",
  "isAvailableInTicketingAndMessaging": "yes",
  "offlineMessageMailTo": "All agents in the department",
  "offlineMessageEmailAddresses": "",
  "agentIds":  [
    68,
    ...,
  ],
  ...
},
...,
]
```

### get a department
  `GET /api/v3/globalSettings/departments/{id}`

#### Parameters

Path parameters

  | Name  | Type | Required  | Description |
  | - | - | - | - |
  |`id` | Guid | yes  |  The id of the department |

Query string

  | Name  | Type | Required  | Default | Description |
  | - | - | - | - | - |
  |`include`|string|no||Available value:`agent`,`shift` |

  #### Response

  The response is a [Department](#department-object) Object.

#### Example
Using curl
```
curl -H "Content-Type: application/json" -X GET https://api1.comm100.io/api/v3/globalSettings/departments/bs22qa68-92e6-4487-a2e8-8234fc9d1f48
```
Response
```json
HTTP/1.1 200 OK
Content-Type:  application/json
{
  "id": "bs22qa68-92e6-4487-a2e8-8234fc9d1f48",
  "name": "markting",
  "description": "markting departments",
  "isAvailableInChat": "yes",
  "isAvailableInTicketingAndMessaging": "yes",
  "offlineMessageMailTo": "All agents in the department",
  "offlineMessageEmailAddresses": "",
  "agentIds":  [
    68,
    ...,
  ],
  ...
}
```


### Create a new department
  `POST /api/v3/globalSettings/departments`

####  Parameters

Request body

  The request body contains data with the [Department](#department-object) structure.

  example:
```json
 {
      "name": "markting",
      "description": "markting departments",
      "isAvailableInChat": "yes",
      "isAvailableInTicketingAndMessaging": "yes",
      "offlineMessageMailTo": "All agents in the department",
      "offlineMessageEmailAddresses": "",
      "agentIds":  [
        68,
        ...,
      ],
      ...
    }
```

#### Response
The response is a [Department](#department-object) object

#### Example
Using curl
```
curl -H "Content-Type: application/json" -d '{
      "name": "markting",
      "description": "markting departments",
      "isAvailableInChat": "yes",
      "isAvailableInTicketingAndMessaging": "yes",
      "offlineMessageMailTo": "All agents in the department",
      "offlineMessageEmailAddresses": "",
      "agentIds":  [
        68,
        ...,
      ],
      ...
    }' -X POST https://api1.comm100.io/api/v3/globalSettings/roles
```
Response
```json
HTTP/1.1 201 Created
Content-Type:  application/json
Location: https://api1.comm100.io/api/v3/globalSettings/departments
{
  "id": "bs22qa68-92e6-4487-a2e8-8234fc9d1f48",
  "name": "markting",
  "description": "markting departments",
  "isAvailableInChat": "yes",
  "isAvailableInTicketingAndMessaging": "yes",
  "offlineMessageMailTo": "All agents in the department",
  "offlineMessageEmailAddresses": "",
  "agentIds":  [
    68,
    ...,
  ],
  ...
}
```



### update a department
  `PUT /api/v3/globalSettings/departments/{id}`

####  Parameters
Path parameters

  | Name  | Type | Required  | Description |
  | - | - | - | - |
  |`id` | Guid | yes  |  The id of the department |

Request body

  The request body contains data with the  [Department](#department-object) structure.

  example:
```json
{
  "name": "markting",
  "description": "markting departments",
  "isAvailableInChat": "yes",
  "isAvailableInTicketingAndMessaging": "yes",
  "offlineMessageMailTo": "All agents in the department",
  "offlineMessageEmailAddresses": "",
  "agentIds":  [
    68,
    ...,
  ],
  ...
}
```

#### Response
The response is a [Department](#department-object) Object.

#### Example
Using curl
```
curl -H "Content-Type: application/json" -d '{
  "name": "markting",
  "description": "markting departments",
  "isAvailableInChat": "yes",
  "isAvailableInTicketingAndMessaging": "yes",
  "offlineMessageMailTo": "All agents in the department",
  "offlineMessageEmailAddresses": "",
  "agentIds":  [
    68,
    ...,
  ],
  ...
}' -X PUT https://api1.comm100.io/api/v3/globalSettings/departments/bs22qa68-92e6-4487-a2e8-8234fc9d1f48
```
Response
```json
HTTP/1.1 200 OK
Content-Type: application/json
Location: https://api1.comm100.io/api/v3/globalSettings/departments/bs22qa68-92e6-4487-a2e8-8234fc9d1f48
{
  "id": "bs22qa68-92e6-4487-a2e8-8234fc9d1f48",
  "name": "markting",
  "description": "markting departments",
  "isAvailableInChat": "yes",
  "isAvailableInTicketingAndMessaging": "yes",
  "offlineMessageMailTo": "All agents in the department",
  "offlineMessageEmailAddresses": "",
  "agentIds":  [
    68,
    ...,
  ],
  ...
}
```

### Delete a department
  `DELETE /api/v3/globalSettings/departments/{id}`

#### Parameters

Path parameters

  | Name  | Type | Required  | Description |
  | - | - | - | - |
  |`id` | Guid | yes  |  the department id |

#### Response
HTTP/1.1 204 No Content

#### Example
Using curl
```
curl -X DELETE https://api1.comm100.io/api/v3/globalSettings/departments/4487fc9d-92e6-4487-a2e8-92e68d6892e6
```
Response
```json
HTTP/1.1 204 No Content
```


# Permission

  Permission is hard-coded.

  + `GET /api/v3/globalSettings/permissions` - [Get all permissions](#get-all-permissions)
  + `GET /api/v3/globalSettings/roles/{roleId}/permissions` - [Get role permissions](#get-role-permissions)
  + `GET /api/v3/globalSettings/agents/{agentId}/permissions` - [Get agent permissions](#get-agent-permissions)
  + `GET /api/v3/globalSettings/agents/{agentId}/permissions:effective` - [Get a list of agent's effective permissions](#get-agent-effective-permissions)
  + `PUT /api/v3/globalSettings/roles/{roleId}/permissions` - [Update role permissions](#update-role-permissions)
  + `PUT /api/v3/globalSettings/agents/{agentId}/permissions` - [Update agent permissions](#update-agent-permissions)


## Permission Related Object JSON format

### Permission Object

  | Name | Type | Include | Read-only | Mandatory | Default | Description |
  | - | - | :-: | :-: | :-: | :-: | - |
  |`id` | long | | no | no |  |  |
  |`name` | string| | yes | no | |  |
  |`description` | string| | yes | no | |  |
  |`category` | string | | yes | no | |The value options include: liveChat, ticketingAndMessaging, bot, globalSettings, knowledgeBase|

## Permission Endpoints

### Get all permission
  `GET /api/v3/globalSettings/permissions`

#### Parameters
    No parameters

#### Response

  The response is a list of [Permission](#permission) Objects

#### Example
  Using curl
  ```
  curl -H "Content-Type: application/json" -X GET https://api1.comm100.io/api/v3/globalSettings/permissions
  ```
  Response
  ```json
  HTTP/1.1 200 OK
  Content-Type:  application/json[
  {
    "id": 201,
    "name": "Accept Chats",
    "description": "Accept Chats",
    "category": "Live Chat",
  },
  ...,
  ]
  ```



### Get role permissions
  `GET /api/v3/globalSettings/roles/{roleId}/permissions`

#### Parameters
Path parameters

  | Name  | Type | Required  | Description |
  | - | - | - | - |
  |`roleId` | Guid | yes  |  The id of the role |

#### Response

The response is a [Permission](#permission) Object.


#### Example
  Using curl
  ```
  curl -H "Content-Type: application/json" -X GET https://api1.comm100.io/api/v3/globalSettings/roles/4487fc9d-92e6-4487-a2e8-92e68d68927777/permissions
  ```
  Response
  ```json
  HTTP/1.1 200 OK
  Content-Type:  application/json[
    {
      "id": 201,
      "name": "Accept Chats",
      "description": "Accept Chats",
      "category": "Live Chat",
    },
    ...,
  ]
  ```


### Get agent permissions
  `GET /api/v3/globalSettings/agents/{agentId}/permissions`

#### Parameters
Path parameters

  | Name  | Type | Required  | Description |
  | - | - | - | - |
  |`agentId` | integer | yes  |  The id of the agent |

  #### Response

  The response is a list of [Permission](#permission) Objects


  #### Example
Using curl
```
curl -H "Content-Type: application/json" -X GET https://api1.comm100.io/api/v3/globalSettings/agents/68/permissions
```
Response
```json
HTTP/1.1 200 OK
Content-Type:  application/json[
  {
    "id": 201,
    "name": "Accept Chats",
    "description": "Accept Chats",
    "category": "Live Chat",
  },
  ...,
]
```

### Get agent effective permissions

  `GET /api/v3/globalSettings/agents/{agentId}/permissions:effective`

#### Parameters
Path parameters

  | Name  | Type | Required  | Description |
  | - | - | - | - |
  |`agentId` | integer | yes  |  The id of the agent |

  #### Response

  The response is a list of [Permission](#permission) Objects


  #### Example
  Using curl
  ```
  curl -H "Content-Type: application/json" -X GET https://api1.comm100.io/api/v3/globalSettings/agents/68/permissions:effective
  ```
  Response
  ```json
  HTTP/1.1 200 OK
  Content-Type:  application/json[
    {
      "id": 201,
      "name": "Accept Chats",
      "description": "Accept Chats",
      "category": "Live Chat",
    },
    ...,
  ]
  ```

### update role permissions

 `PUT /api/v3/globalSettings/roles/{roleId}/permissions`

####  Parameters
Path parameters

  | Name  | Type | Required  | Description |
  | - | - | - | - |
  |`roleId` | Guid | yes  |  The id of the role |

Request body

  The request body contains data with the  [Permission](#permission-object) Id list

  example:
```json
[
  201,
  205,
  ...,
]
```

#### Response
The response is a role [Permission](#permission-object) Object list.

#### Example
Using curl
```
curl -H "Content-Type: application/json" -d '[
  201,
  205,
  ...,
]' -X PUT https://api1.comm100.io/api/v3/globalSettings/roles/4487fc9d-92e6-4487-a2e8-92e68d68927777/permissions
```
Response
```json
HTTP/1.1 201 Created
Content-Type:  application/json
Location: https://api1.comm100.io/api/v3/globalSettings/roles/4487fc9d-92e6-4487-a2e8-92e68d68927777/permissions
[{
    "id": 201,
    "name": "Accept Chats",
    "description": "Accept Chats",
    "category": "Live Chat",
  },
  ...,
]
```

### update agent permissions
`PUT /api/v3/globalSettings/agents/{agentId}/permissions`

####  Parameters
Path parameters

  | Name  | Type | Required  | Description |
  | - | - | - | - |
  |`agentId` | integer | yes  |  The id of the agent |

Request body

  The request body contains data with the  [Permission](#permission-object) Id list

  example:
```json
[
  201,
  205,
  ...,
]
```

#### Response
The response is: agent [Permission](#permission-object) Object list.

#### Example
Using curl
```
curl -H "Content-Type: application/json" -d '[
  201,
  205,
  ...,
]' -X PUT https://api1.comm100.io/api/v3/globalSettings/agents/68/permissions
```
Response
```json
HTTP/1.1 201 Created
Content-Type:  application/json
Location: https://api1.comm100.io/api/v3/globalSettings/agents/68/permissions
[{
    "id": 201,
    "name": "Accept Chats",
    "description": "Accept Chats",
    "category": "Live Chat",
  },
  ...,
]
```



# Shift

You need `Manage Security` permission to set shift for a site.

- `GET /api/v3/globalSettings/shifts` - [Get all shifts in site](#get-all-shifts-in-site)
- `GET /api/v3/globalSettings/departments/{departmentId}/shifts` - [Get all shifts by department id](#get-all-shifts-by-department-id)
- `GET /api/v3/globalSettings/agents/{agentId}/shifts` - [Get all shifts by agent id](#get-all-shifts-by-agent-id)
- `GET /api/v3/globalSettings/shifts/{id}` - [Get a shift by id](#get-a-shift-by-id)
- `POST /api/v3/globalSettings/shifts` - [create a new shift](#create-a-new-shift)
- `PUT /api/v3/globalSettings/shifts/{id}` - [update a shift](#update-a-shift)
- `DELETE /api/v3/globalSettings/shifts/{id}` - [delete a shift](#delete-a-shift)

## Related Object JSON format

### Shift Object 

  Shift Object is represented as simple flat JSON objects with the following keys:

  | Name | Type | Include | Read-only | Mandatory | Default | Description |
  | - | - |- | :-: | :-: | :-: | - |
  | `id` | Guid | | yes | no | | Id of the current item.  |
  | `name` | string  | | no | yes | | Name of the shift. |
  | `timeZone` | string  | | no | no | | Time zone of shift. value include all [Time Zone Option](#time-zone-options) Ids. |
  | `ifAutoDetectDayLight` | boolean  | | no | no | |  |
  | `holidays` | [Holiday](#holiday-object)[]  | | no | no | | |
  | `agentIds` | int[] | | yes | no | NULL | |
  | `departmentIds` | Guid[] | | yes | no | NULL | |
  | `agents` | [Agent](#Agent-Object)[] | yes | no | no | | |
  | `departments` | [Department](#Department-Object)[] | yes | no | no | | |
  | `workingHours` | [Working Hours](#Working-Hours-Object)[]  | | no | no | | |

### Holiday Object

  Holiday Object is represented as simple flat JSON objects with the following keys:

  | Name | Type | Read-only | Mandatory | Default | Description |
  | - | - | :-: | :-: | :-: | - |
  | `name` | string  | no | yes | | The name of holiday. |
  | `date` | DateTime  | no | yes | | The date of the holiday. |


### Working Hours Object

  Working Hours Object is represented as simple flat JSON objects with the following keys:

  | Name | Type | Read-only | Mandatory | Default | Description |
  | - | - | :-: | :-: | :-: | - |
  | `dayOfWeek` | string  | no | yes | | Including `monday`, `tuesday`, `wednesday`, `thursday`, `friday`, `saturday` and `sunday`. |
  | `startTime` | time  | no | yes | | |
  | `endTime` | time  | no | yes | | |
  | `awayStatusId` | Guid | no | no | | |

## Shift Endpoints

### Get all Shifts in site

  `GET /api/v3/globalSettings/shifts`

#### Parameters

Query string

  | Name  | Type | Required  | Default | Description |
  | - | - | - | - | - |
  | `include` | string | no  |  | Available value: `department`, `agent` |

#### Response

The response is a list of [Shift](#shift-object) Objects.

#### Example

Using curl
```
curl -H "Content-Type: application/json"
-X GET https://api1.comm100.io/api/v3/globalSettings/shifts?include=department
```

Response
``` json
HTTP/1.1 200 OK
Content-Type:  application/json

[
  {
    "id": "3964B5AE-6DAD-D774-BFCB-8C1F6B58ACED",
    "name": "Shifts",
    "timeZone": "(GMT) Coordinated Universal Time",
    "ifAutoDetectDayLight": false,
    "holidays": [{
      "name": "summary",
      "date": "2019-11-11"
    },
    ...
    ],
    "agentIds": [68],
    "departmentIds": ["1DC43077-E36F-F9EA-C7BA-C29620102F7E"],
    "departments": [{// include department
      "id": "1DC43077-E36F-F9EA-C7BA-C29620102F7E",
      "name": "departments",
      "siteId": "FEF12049-BF08-2CD4-C405-A5FC8AE75D0F",
      "description": "departments",
      "isAvailableInChat": false,
      "isAvailableInTicketingAndMessaging": false,
      "offlineMessageMailTo": "The email address(es)",
      "offlineMessageEmailAddresses": "test@comm100.com",
      "agentId": 68,
      ...
      },
      ...,
    ],
    "workingHours": [{
      "dayOfWeek": "sunday",
      "startTime": "12:00",
      "endTime": "14:00",
      "awayStatusId": "BAACB779-2E41-27C5-B23D-1C8F2058862D"
      },
      ...
    ]
  },
  ...
]
```

### Get a Shift by id

  `GET /api/v3/livechat/shifts/{id}`

#### Parameters

Path Parameters

  | Name  | Type | Required  | Description |
  | - | - | - | - |
  | `id` | Guid | yes  |  The unique id of the shift |

Query string

  | Name  | Type | Required  | Default | Description |
  | - | - | - | - | - |
  | `include` | string | no  |  | Available value: `department`, `agent` |

#### Response

The response is a [Shift](#shift-object) Object.

#### Example

Using curl
```
curl -H "Content-Type: application/json"
-X GET https://api1.comm100.io/api/v3/globalSettings/shifts/3964B5AE-6DAD-D774-BFCB-8C1F6B58ACED?include=department
```

Response
``` json
HTTP/1.1 200 OK
Content-Type:  application/json

{
  "id": "3964B5AE-6DAD-D774-BFCB-8C1F6B58ACED",
  "name": "Shifts",
  "timeZone": "(GMT) Coordinated Universal Time",
  "ifAutoDetectDayLight": false,
  "holidays": [{
    "name": "summary",
    "date": "2019-11-11"
  },
  ...
  ],
  "agentIds": [68],
  "departmentIds": ["1DC43077-E36F-F9EA-C7BA-C29620102F7E"],
  "departments": [{// include department
    "id": "1DC43077-E36F-F9EA-C7BA-C29620102F7E",
    "name": "departments",
    "siteId": "FEF12049-BF08-2CD4-C405-A5FC8AE75D0F",
    "description": "departments",
    "isAvailableInChat": false,
    "isAvailableInTicketingAndMessaging": false,
    "offlineMessageMailTo": "The email address(es)",
    "offlineMessageEmailAddresses": "test@comm100.com",
    "agentId": 68,
    ...
    },
    ...
  ],
  "workingHours": [{
    "dayOfWeek": "sunday",
    "startTime": "12:00",
    "endTime": "14:00",
    "awayStatusId": "BAACB779-2E41-27C5-B23D-1C8F2058862D"
    },
    ...
  ]
}
```

### Get all Shifts by department id

  `GET /api/v3/globalSettings/departments/{departmentId}/shifts`

#### Parameters

Path Parameters

  | Name  | Type | Required  | Description |
  | - | - | - | - |
  | `departmentId` | Guid | yes  |  The id of the department |

#### Response

The response is a [Shift](#shift-object) Object.

#### Example

Using curl
```
curl -H "Content-Type: application/json"
-X GET https://api1.comm100.io/api/v3/globalSettings/departments/1DC43077-E36F-F9EA-C7BA-C29620102F7E/shifts
```

Response
``` json
HTTP/1.1 200 OK
Content-Type:  application/json

[
  {
    "id": "3964B5AE-6DAD-D774-BFCB-8C1F6B58ACED",
    "name": "Shifts",
    "timeZone": "(GMT) Coordinated Universal Time",
    "ifAutoDetectDayLight": false,
    "holidays": [{
      "name": "summary",
      "date": "2019-11-11"
    },
    ...
    ],
    "agentIds": [68],
    "departmentIds": ["1DC43077-E36F-F9EA-C7BA-C29620102F7E"],
    "workingHours": [{
      "dayOfWeek": "sunday",
      "startTime": "12:00",
      "endTime": "14:00",
      "awayStatusId": "BAACB779-2E41-27C5-B23D-1C8F2058862D"
    },
    ...
    ]
  },
  ...
]
```

### Get all Shifts by agent id

  `GET /api/v3/globalSettings/agents/{agentId}/shifts`

#### Parameters

Path Parameters

  | Name  | Type | Required  | Description |
  | - | - | - | - |
  | `agentId` | integer | yes  |  the unique Id of the agent |

#### Response

The response is a [Shift](#shift-object) Object.

#### Example

Using curl
```
curl -H "Content-Type: application/json"
-X GET https://api1.comm100.io/api/v3/globalSettings/agents/68/shifts
```

Response
``` json
HTTP/1.1 200 OK
Content-Type:  application/json

[
  {
    "id": "3964B5AE-6DAD-D774-BFCB-8C1F6B58ACED",
    "name": "Shifts",
    "timeZone": "(GMT) Coordinated Universal Time",
    "ifAutoDetectDayLight": false,
    "holidays": [{
      "name": "summary",
      "date": "2019-11-11"
    },
    ...
    ],
    "agentIds": [68],
    "departmentIds": ["1DC43077-E36F-F9EA-C7BA-C29620102F7E"],
    "workingHours": [{
      "dayOfWeek": "sunday",
      "startTime": "12:00",
      "endTime": "14:00",
      "awayStatusId": "BAACB779-2E41-27C5-B23D-1C8F2058862D"
    },
    ...
    ]
  },
  ...
]
```

### Create a new Shift

  `POST /api/v3/globalSettings/shifts`

#### Parameters

Request Body

  The request body contains data with the [Shift](#shift-object) structure.

example:
```Json
{
  "name": "Shifts1111",
  "timeZone": "UTC",
  "ifAutoDetectDayLight": false,
  "holidays": [{
    "name": "summary",
    "date": "2019-11-11"
  },
  ...
  ],
  "agentIds": [68],
  "departmentIds": ["1DC43077-E36F-F9EA-C7BA-C29620102F7E"],
  "workingHours": [{
    "dayOfWeek": "sunday",
    "startTime": "12:00",
    "endTime": "14:00",
    "awayStatusId": "BAACB779-2E41-27C5-B23D-1C8F2058862D"
    },
    ...
  ]
}
```

#### Response

The response is a [Shift](#shift-object) Object.

#### Example

Using curl
```
curl -H "Content-Type: application/json" -d '{
  "name": "Shifts1111",
  "timeZone": "UTC",
  "ifAutoDetectDayLight": false,
  "holidays": [{
    "name": "summary",
    "date": "2019-11-11"
  },
  ...
  ],
  "agentIds": [68],
  "departmentIds": ["1DC43077-E36F-F9EA-C7BA-C29620102F7E"],
  "workingHours": [{
    "dayOfWeek": "sunday",
    "startTime": "12:00",
    "endTime": "14:00",
    "awayStatusId": "BAACB779-2E41-27C5-B23D-1C8F2058862D"
    },
    ...
  ]
  }' -X POST https://api1.comm100.io/api/v3/globalSettings/shifts
```

Response
``` json
HTTP/1.1 201 Created
Content-Type:  application/json
Location: https://api1.comm100.io/api/v3/globalSettings/shifts/3964B5AE-6DAD-D774-BFCB-8C1F6B58ACED

{
  "id": "3964B5AE-6DAD-D774-BFCB-8C1F6B58ACED",
  "name": "Shifts",
  "timeZone": "(GMT) Coordinated Universal Time",
  "ifAutoDetectDayLight": false,
  "holidays": [{
    "name": "summary",
    "date": "2019-11-11"
  },
  ...
  ],
  "agentIds": [68],
  "departmentIds": ["1DC43077-E36F-F9EA-C7BA-C29620102F7E"],
  "workingHours": [{
    "dayOfWeek": "sunday",
    "startTime": "12:00",
    "endTime": "14:00",
    "awayStatusId": "BAACB779-2E41-27C5-B23D-1C8F2058862D"
    },
    ...
  ]
}
```

### Update a Shift

  `PUT /api/v3/globalSettings/shifts/{id}`

#### Parameters

Path Parameters

  | Name  | Type | Required  | Description |
  | - | - | - | - |
  | `id` | Guid | yes  |  the unique Id of the shift |

Request Body

  The request body contains data with the [Shift](#shift-object) structure.

example:
```Json
{
  "name": "Shifts2222",
  "timeZone": "UTC",
  "ifAutoDetectDayLight": false,
  "holidays": [{
    "name": "summary",
    "date": "2019-11-11"
  },
  ...
  ],
  "agentIds": [68],
  "departmentIds": ["1DC43077-E36F-F9EA-C7BA-C29620102F7E"],
  "workingHours": [{
    "dayOfWeek": "sunday",
    "startTime": "12:00",
    "endTime": "14:00",
    "awayStatusId": "BAACB779-2E41-27C5-B23D-1C8F2058862D"
    },
    ...
  ]
}
```

#### Response

The response is a [Shift](#shift-object) Object.

#### Example

Using curl
```
curl -H "Content-Type: application/json" -d '{
  "name": "Shifts2222",
  "timeZone": "UTC",
  "ifAutoDetectDayLight": false,
  "holidays": [{
    "name": "summary",
    "date": "2019-11-11"
  },
  ...
  ],
  "agentIds": [68],
  "departmentIds": ["1DC43077-E36F-F9EA-C7BA-C29620102F7E"],
  "workingHours": [{
    "dayOfWeek": "sunday",
    "startTime": "12:00",
    "endTime": "14:00",
    "awayStatusId": "BAACB779-2E41-27C5-B23D-1C8F2058862D"
    },
    ...
  ]
  }' -X PUT https://api1.comm100.io/api/v3/globalSettings/shifts/3964B5AE-6DAD-D774-BFCB-8C1F6B58ACED
```

Response
``` json
HTTP/1.1 200 OK
Content-Type:  application/json

{
  "id": "3964B5AE-6DAD-D774-BFCB-8C1F6B58ACED",
  "name": "Shifts2222",
  "timeZone": "(GMT) Coordinated Universal Time",
  "ifAutoDetectDayLight": false,
  "holidays": [{
    "name": "summary",
    "date": "2019-11-11"
  },
  ...
  ],
  "agentIds": [68],
  "departmentIds": ["1DC43077-E36F-F9EA-C7BA-C29620102F7E"],
  "workingHours": [{
    "dayOfWeek": "sunday",
    "startTime": "12:00",
    "endTime": "14:00",
    "awayStatusId": "BAACB779-2E41-27C5-B23D-1C8F2058862D"
    },
    ...
  ]
}
```

### Delete a Shift

  `DELETE /api/v3/globalSettings/shifts/{id}`

#### Parameters

Path Parameters

  | Name  | Type | Required  | Description |
  | - | - | - | - |
  | `id` | Guid | yes  |  the unique Id of the shift |

#### Response

HTTP/1.1 204 No Content

#### Example

Using curl
```
curl -X DELETE https://api1.comm100.io/api/v3/globalSettings/shifts/3964B5AE-6DAD-D774-BFCB-8C1F6B58ACED
```

Response
```Json
  HTTP/1.1 204 No Content
```

# Contact
 + `GET /api/v3/globalSettings/contacts` - [Get a list of contacts in site](#get-all-contacts)
 + `GET /api/v3/globalSettings/contacts/{id}` - [Get a contact by id](#get-an-contact)
 + `POST /api/v3/globalSettings/contacts` - [Create a new contact](#create-a-new-contact)
 + `PUT /api/v3/globalSettings/contacts/{id}` - [Update a contact](#update-a-contact)
 + `DELETE /api/v3/globalSettings/contacts/{id}` - [Delete a contact](#delete-a-contact)

## Contact Related Object JSON format

### Contact Object

  | Name | Type | Include | Read-only | Mandatory | Default | Description |
  | - | - | :-: | :-: | :-: | :-: | - |
  |`id` | integer  | | yes | no |  |  |
  |`name` | string  | | no | yes | | Contact Name can be edited by Agents. Default value is read from the first Identity. Only when a Contact sends a message in a specific channel that has a Name and an Avatar, like a Facebook Account, will the display Name and Avatar be used instead of the drawn from the contact profile. In all other situations, display will be drawn from the Contact Name and Avatar.|
  |`description` | string | | no | no | |  |
  |`firstName` | string | | no | no | |  |
  |`lastName` | string | | no | no | |  |
  |`alias` | string | | no | no | |  |
  |`title` | string | | no | no | |  |
  |`company` | string  | | no | no | |  |
  |`fax` | string  | | no | no | |  |
  |`phone` | string  | | no | no | | |
  |`mailingAddress` | string  | | no | no | |  |
  |`city` | string | | no | no | |  |
  |`stateOrProvince` | string  | | no | no | |  |
  |`countryOrRegion` | string  | | no | no | |  |
  |`postalOrZipCode` | string  | | no | no | |  |
  |`timeZone` | string | | no | no | |  Time zone of contact. value includes all [Time Zone Option](#time-zone-options) Ids.|
  |`createdTime` | DateTime | | no | no | | When the contact is created.|
  |`lastUpdatedTime` | DateTime | | no | no | |  |
  |`contactIdentities` | [ContactIdentity](#Contact-Identity)[] | yes | no | no | | Contact Identities. |

### Contact List Response Object

  Agent List Object for agent list Response, include count and page information.

  | Name | Type | Include | Read-only | Mandatory | Default | Description |
  | - | - | :-: | :-: | :-: | :-: | - |
  |`count` | integer  | no | yes | no | | The total count of the query  |
  |`nextPage` | string  | no | yes | no | | The next page url of the query  |
  |`previousPage` | string  | no | yes | no | | The previous page url of the query  |
  |`contacts`|   [Contact](#contact-object)[]| no | yes| no | | A list of contacts. |



## Contact Endpoints


### Get all contacts
  `GET /api/v3/globalSettings/contacts`

#### Parameters
   Query string

  | Name  | Type | Required  | Default | Description |
  | - | - | - | - | - |
  |`name` | string | no  |  | Contact name. |
  |`company`|string|no|  | Contact company. |
  |`contactIdentityName` | string | no  |  | Contact identity name. |
  |`contactIdentityValue` | string | no  |  | Contact identity value. |
  |`contactIdentityType` | string | no  |  | Contact identity type. |
  |`keywords` | string | no  |  | Search scope includes: name, company, identity value, alias. |
  |`include` | string | no  |  | Available value: `contactIdentity` |
  |`pageIndex`|integer|no| 1 | The page index of the query. |
  |`pageSize`|integer|no| 10 | The page size of the query. |



  #### Response
  The response is a [Contact List Response](#contact-list-response-object) Object.


#### Example
  Using curl
  ```
  curl -H "Content-Type: application/json" -X GET https://api1.comm100.io/api/v3/globalSettings/contacts?name=Vincent
  ```
  Response
  ```json
  HTTP/1.1 200 OK
  Content-Type:  application/json
  {
    "count": 1234,
    "nextPage": "https://api1.comm100.io/api/v3/globalSettings/contacts?name=Vincent&pageIndex=2",
    "previousPage": "",
    "contacts": [{
      "id": 7,
      "name": "Vincent",
      "description": "Accept Chats",
      "firstName": "Vincent",
      "lastName": "Crabbe",
      "alias": "",
      "title": "CEO",
      "company": "BMW",
      ...
    },
    ...
    ]
}
  ```


### Get a contact
  `GET /api/v3/globalSettings/contacts/{id}`

#### Parameters

Path parameters

  | Name  | Type | Required  | Description |
  | - | - | - | - |
  |`id` | integer | yes  |  The id of the contact |

   Query string

  | Name  | Type | Required  | Default | Description |
  | - | - | - | - | - |
  |`include` | string | no  |  | Available value: `contactIdentity` |


#### Response
  The response is a [Contact](#contact-object) Object.

#### Example
Using curl
```
curl -H "Content-Type: application/json" -X GET https://api1.comm100.io/api/v3/globalSettings/contacts/7
```
Response
```json
HTTP/1.1 200 OK
Content-Type:  application/json
{
  "id": 7,
  "name": "Vincent",
  "description": "Accept Chats",
  "firstName": "Vincent",
  "lastName": "Crabbe",
  "alias": "",
  "title": "CEO",
  "company": "BMW",
  ...
}
```

### Create a new contact

  `POST /api/v3/globalSettings/contacts`

####  Parameters

Request body

  The request body contains data with the [Contact](#contact-object) structure.

example:
```json
{
      "name": "Vincent",
      "description": "Accept Chats",
      "firstName": "Vincent",
      "lastName": "Crabbe",
      "alias": "",
      "title": "CEO",
      "company": "BMW",
      ...
    }
```

#### Response

The response is a [Contact](#contact-object) Object.

#### Example
Using curl
```
curl -H "Content-Type: application/json" -d '{
      "name": "Vincent",
      "description": "Accept Chats",
      "firstName": "Vincent",
      "lastName": "Crabbe",
      "alias": "",
      "title": "CEO",
      "company": "BMW",
      ...
    }' -X POST https://api1.comm100.io/api/v3/globalSettings/contacts
```
Response
```json
HTTP/1.1 201 Created
Content-Type:  application/json
Location: https://api1.comm100.io/api/v3/globalSettings/contacts/7
{
  "id": 7,
  "name": "Vincent",
  "description": "Accept Chats",
  "firstName": "Vincent",
  "lastName": "Crabbe",
  "alias": "",
  "title": "CEO",
  "company": "BMW",
  ...
}
```

### Update a contact
  `PUT /api/v3/globalSettings/contacts/{id}`

####  Parameters

Path parameters

  | Name  | Type | Required  | Description |
  | - | - | - | - |
  |`id` | integer | yes  |  The id of the contact |

Request body

  The request body contains data with the [Contact](#contact-object) structure.

  example:
```json
{
  "name": "Vincent",
  "description": "Accept Chats",
  "firstName": "Vincent",
  "lastName": "Crabbe",
  "alias": "",
  "title": "CEO",
  "company": "BMW",
  ...
}
```

#### Response
The response is a [Contact](#contact-object) Object.

#### Example
Using curl
```
curl -H "Content-Type: application/json" -d '{
  "name": "Vincent",
  "description": "Accept Chats",
  "firstName": "Vincent",
  "lastName": "Crabbe",
  "alias": "",
  "title": "CEO",
  "company": "BMW",
  ...
}' -X PUT https://api1.comm100.io/api/v3/globalSettings/contacts
```
Response
```json
HTTP/1.1 200 OK
Content-Type: application/json
Location: https://api1.comm100.io/api/v3/globalSettings/contacts/7
{
    "id": 7,
    "email": "Tom@gmail.com",
    "displayName":"Tom",
    "firstName":"Tom",
    "lastName":"Green",
    "title":"CEO",
    ...
}
```


### Delete a contact
  `DELETE /api/v3/globalSettings/contacts/{id}`

#### Parameters

Path parameters

  | Name  | Type | Required  | Description |
  | - | - | - | - |
  |`id` | integer | yes  | The contact id |

#### Response
HTTP/1.1 204 No Content

#### Example
Using curl
```
curl -X DELETE https://api1.comm100.io/api/v3/globalSettings/contacts/7
```

Response
```json
HTTP/1.1 204 No Content
```

# Contact Identity
 + `GET /api/v3/globalSettings/contacts/{contactId}/contactIdentities` - [Get a list of contact identities in a contact](#get-all-contact-identity)
 + `GET /api/v3/globalSettings/contactIdentities/{id}` - [Get an contact identity by id](#get-an-contact-identity)
 + `POST /api/v3/globalSettings/contactIdentities` - [create a new contact identity](#create-a-new-contact-identity)
 + `PUT /api/v3/globalSettings/contactIdentities/{id}` - [update an contact identity](#update-an-contact-identity)
 + `DELETE /api/v3/globalSettings/contactIdentities/{id}` - [delete an contact identity](#delete-an-contact-identity)

 ## Contact Identity Related Object JSON format

### Contact Identity Object

  | Name | Type | Include | Read-only | Mandatory | Default | Description |
  | - | - | :-: | :-: | :-: | :-: | - |
  |`id` | integer| | yes | no | |  |
  |`contactId` | integer| | no | yes | | Mandatory when post by contact identity api. |
  |`name` | string | | no | no | | The name used for a specific channel, like the name of a Facebook user. Not every channel will yield a name, for example, SMS Numbers will display a number instead of a name.|
  |`type` | string| | no | yes | | The options for the value are: visitor, email, SMS, Facebook, Twitter, WeChat, ssoid, externalid, Whatsapp, inphase.|
  |`value` | string  | | no | yes | | The value of the identity.|
  |`avatarURL` | string | | no | no | | The avatar used in a certain channel, like the avatar of a Facebook user. Not every channel yields an avatar, for example, SMS Numbers won't produce one.|
  |`infoURL` | string  | | no | no | | Contact information from the channels. Such as the number of Twitter followers, tweets from the twitter identity. The info is displayed in an iframe in the agent console. Available for Twitter, Facebook, SMS, WeChat.|
  |`screenName` | string | | no | no | | Twitter only. Like @Comm100Corp.|
  |`originalContactPageURL` | string | | no | no | | The contact profile URL on Facebook or Twitter.|

## Contact Identity Endpoints

### Get all contact identity

  `GET /api/v3/globalSettings/contacts/{contactId}/contactIdentities`

#### Parameters

Path parameters

  | Name  | Type | Required  | Description |
  | - | - | - | - |
  |`contactId` | integer | yes  |  The id of the contact of contact Identities belong|


#### Response

  The response is a list of [Contact Identity](#contact-identity-object) Objects

#### Example
Using curl
```
curl -H "Content-Type: application/json" -X GET https://api1.comm100.io/api/v3/globalSettings/contacts/7/contactIdentities
```
Response
```json
HTTP/1.1 200 OK
Content-Type:  application/json[
{
  "id": 25,
  "contactId": 7,
  "name": "Vincent",
  "type": "Visitor",
  "value": "",
  "avatarURL": "https://bot.comm100.com/api/v3/chatbot/images/42dwdaww-92e6-4487-a2e8-92e68d6892e6",
  "infoURL": "",
  "screenName": "@Comm100Corp",
  "originalContactPageURL": ""
},
...,
]
```



### Get a contact identity
  `GET /api/v3/globalSettings/contactIdentities/{id}`

#### Parameters
Path parameters

  | Name  | Type | Required  | Description |
  | - | - | - | - |
  |`id` | integer | yes  |  The id of the contact Identitie |

#### Response

  The response is a [Contact Identity](#contact-identity-object) Object.

#### Example

Using curl
```
curl -H "Content-Type: application/json" -X GET https://api1.comm100.io/api/v3/globalSettings/contactIdentities/25
```
Response
```json
HTTP/1.1 200 OK
Content-Type:  application/json
{
  "id": 25,
  "contactId": 7,
  "name": "Vincent",
  "type": "Visitor",
  "value": "",
  "avatarURL": "https://bot.comm100.com/api/v3/chatbot/images/42dwdaww-92e6-4487-a2e8-92e68d6892e6",
  "infoURL": "",
  "screenName": "@Comm100Corp",
  "originalContactPageURL": ""
}
```


### Create a new contact identity
  `POST /api/v3/globalSettings/contacts/contactIdentities`

####  Parameters

Request body

  The request body contains data with the [Contact Identity](#contact-identity-object) structure.

  example:
```json
 {
    "contactId": 7,
    "name": "Vincent",
    "type": "Visitor",
    "value": "",
    "avatarURL": "https://bot.comm100.com/api/v3/chatbot/images/42dwdaww-92e6-4487-a2e8-92e68d6892e6",
    "infoURL": "",
    "screenName": "@Comm100Corp",
    "originalContactPageURL": ""
  }
```

#### Response

The response is a [Contact Identity](#contact-identity-object) Object.

#### Example

Using curl
```
curl -H "Content-Type: application/json" -d ' {
    "contactId": 7,
    "name": "Vincent",
    "type": "Visitor",
    "value": "",
    "avatarURL": "https://bot.comm100.com/api/v3/chatbot/images/42dwdaww-92e6-4487-a2e8-92e68d6892e6",
    "infoURL": "",
    "screenName": "@Comm100Corp",
    "originalContactPageURL": ""
  }' -X POST https://api1.comm100.io/api/v3/globalSettings/contacts/contactIdentities
```
Response
```json
HTTP/1.1 201 Created
Content-Type:  application/json
Location: https://api1.comm100.io/api/v3/globalSettings/contactIdentities/25
{
    "id": 25,
    "contactId": 7,
    "name": "Vincent",
    "type": "Visitor",
    "value": "",
    "avatarURL": "https://bot.comm100.com/api/v3/chatbot/images/42dwdaww-92e6-4487-a2e8-92e68d6892e6",
    "infoURL": "",
    "screenName": "@Comm100Corp",
    "originalContactPageURL": ""
  }
```

### Update a contact identity

  `PUT /api/v3/globalSettings/contactIdentities/{id}`

####  Parameters

Path parameters

  | Name  | Type | Required  | Description |
  | - | - | - | - |
  |`id` | integer | yes  |  The id of the contact Identitie |

Request body

  The request body contains data with the [Contact Identity](#contact-identity-object) structure.

  example:
```json
 {
    "contactId": 7,
    "name": "Vincent",
    "type": "Visitor",
    "value": "",
    "avatarURL": "https://bot.comm100.com/api/v3/chatbot/images/42dwdaww-92e6-4487-a2e8-92e68d6892e6",
    "infoURL": "",
    "screenName": "@Comm100Corp",
    "originalContactPageURL": ""
  }
```

#### Response

The response is a [Contact Identity](#contact-identity-object) Object.

#### Example
Using curl
```
curl -H "Content-Type: application/json" -d ' {
    "contactId": 7,
    "name": "Vincent",
    "type": "Visitor",
    "value": "",
    "avatarURL": "https://bot.comm100.com/api/v3/chatbot/images/42dwdaww-92e6-4487-a2e8-92e68d6892e6",
    "infoURL": "",
    "screenName": "@Comm100Corp",
    "originalContactPageURL": ""
  }' -X PUT https://api1.comm100.io/api/v3/globalSettings/contactIdentities/25
```
Response
```json
HTTP/1.1 200 OK
Content-Type: application/json
Location: https://api1.comm100.io/api/v3/globalSettings/contactIdentities/25
 {
    "id": 25,
    "contactId": 7,
    "name": "Vincent",
    "type": "Visitor",
    "value": "",
    "avatarURL": "https://bot.comm100.com/api/v3/chatbot/images/42dwdaww-92e6-4487-a2e8-92e68d6892e6",
    "infoURL": "",
    "screenName": "@Comm100Corp",
    "originalContactPageURL": ""
  }
```

### Delete a contact identity
  `DELETE /api/v3/globalSettings/contactIdentities/{id}`

#### Parameters

Path parameters

  | Name  | Type | Required  | Description |
  | - | - | - | - |
  |`id` | integer | yes  |  the contact identity id |

#### Response
HTTP/1.1 204 No Content

#### Example
Using curl
```
curl -X DELETE https://api1.comm100.io/api/v3/globalSettings/contactIdentities/25
```
Response
```json
HTTP/1.1 204 No Content
```

# Visitor

  You need `Manage Settings` permission to manage visitor.

- `GET /api/v3/globalSettings/visitors` - [Get all visitors in site](#get-all-visitors-in-site)
- `GET /api/v3/globalSettings/visitors/{id}` - [Get a visitor by id](#get-a-visitor-by-id)

## Related Object JSON format

### Visitor Object

  Visitor Object is represented as simple flat JSON objects with the following keys:

  | Name | Type | Read-only | Mandatory | Default | Description |
  | - | - | :-: | :-: | :-: | - |
  | `id` | Guid | yes | no | | Id of the current item.  |
  | `name` | string  | no | no | | Name of the visitor. |
  | `email` | string  | no | no | | Email of the visitor. |
  | `numberOfVisits` | integer  | no | no | | The total number of web pages the visitor viewed on your website. |
  | `numberOfChats` | integer  | no | no | | The total times of chats a visitor has made on your website from the first time to present. |
  | `firstVisitTime` | datetime  | no | no | | The time the visitor first visited a web page pasted with Comm100 Live Chat code. |

## Visitor Endpoints

### Get all visitors in site

  `GET /api/v3/globalSettings/visitors`

#### Parameters

Query string

| Name  | Type | Required | Default | Description |
| - | - | :-: | :-: | - |
| `requestedTime` | datetime | no  | today |  The time range of query time, defaults to today, format as `yyyy-MM-ddTHH:mm:ss`. |
| `pageIndex` | integer | no  | 1 | The page index of query. |
| `pageSize` | integer | no  | 50 | Page size.  |
| `keywords` | string | no  |  | Filter by keywords in visitor name, email address. |

#### Response

The response body contains data with the follow structure:

| Name | Type | Required | Default | Description |
| - | - | :-: | :-: | - |
| `totalCount` | integer | no | no | Total count of the list. |
| `previousPage` | string | no | no | Url of the previous page. |
| `nextPage` | string | no | no | Url of the next page. |
| `visitors` | [Visitor](#visitor-Object)[] | no | no |  |

#### Example

Using curl
```
curl -H "Content-Type: application/json"
-X GET https://api1.comm100.io/api/v3/globalSettings/visitors
```

Response
``` json
HTTP/1.1 200 OK
Content-Type:  application/json

{
    "totalCount": 28,
    "previousPage": "",
    "nextPage": "/api/v3/globalSettings/visitors?pageIndex=2",
    "visitors": [{
        "id": "7273e957-02cb-4c03-a84c-44283fcfd47d",
        "name": "test",
        "email": "",
        "numberOfVisits": 4,
        "numberOfChats": 1,
        "firstVisitTime": "2019-06-12T07:41:40.486Z"
    },
    ...
    ]
}
```

### Get a visitor by id

  `GET /api/v3/globalSettings/visitors/{id}`

#### Parameters

Path Parameters

  | Name  | Type | Required  | Description |
  | - | - | - | - |
  | `id` | Guid | yes |  The id of the visitor |

#### Response

The response is a [Visitor](#visitor-object) object.

#### Example

Using curl
```
curl -H "Content-Type: application/json"
-X GET https://api1.comm100.io/api/v3/globalSettings/visitors/7273e957-02cb-4c03-a84c-44283fcfd47d
```

Response
``` json
HTTP/1.1 200 OK
Content-Type:  application/json

{
  "id": "7273e957-02cb-4c03-a84c-44283fcfd47d",
  "name": "test",
  "email": "",
  "numberOfVisits": 4,
  "numberOfChats": 1,
  "firstVisitTime": "2019-06-12T07:41:40.486Z"
}
```

# Public Canned Message Category

  You need `Manage Public Canned Messages` permission to manage the canned message category.

- `GET /api/v3/globalSettings/publicCannedMessageCategories` - [Get all public canned message categories in site](#get-all-Public-Canned-Message-Categories-in-site)
- `GET /api/v3/globalSettings/publicCannedMessageCategories/{id}` - [Get a public canned message category by id](#get-a-Public-Canned-Message-Category-by-id)
- `POST /api/v3/globalSettings/publicCannedMessageCategories` - [create a new public canned message category](#create-a-new-Public-Canned-Message-Category)
- `PUT /api/v3/globalSettings/publicCannedMessageCategories/{id}` - [update a public canned message category](#update-a-Public-Canned-Message-Category)
- `DELETE /api/v3/globalSettings/publicCannedMessageCategories/{id}` - [delete a public canned message category](#delete-a-Public-Canned-Message-Category)

## Related Object JSON format

### Public Canned Message Category Object

  Public Canned Message Category Object is represented as simple flat JSON objects with the following keys:

  | Name | Type | Read-only | Mandatory | Default | Description |
  | - | - | :-: | :-: | :-: | - |
  | `id` | Guid | yes | no | | Id of the current item.  |
  | `name` | string  | no | yes | | Name of the canned message category. |
  | `parentId` | Guid | no | yes | | Id of the public canned message category. If it does not have parent category, parentId is '00000000-0000-0000-0000-000000000000'|

## Public Canned Message Endpoints

### Get all Public Canned Message Categories in site

  `GET /api/v3/globalSettings/publicCannedMessageCategories`

#### Parameters

    No parameters

#### Response

The response is an array of [Public Canned Message Category](#Public-Canned-Message-Category-Object) objects.

#### Example

Using curl
```
curl -H "Content-Type: application/json"
-X GET https://api1.comm100.io/api/v3/globalSettings/publicCannedMessageCategories
```

Response
``` json
HTTP/1.1 200 OK
Content-Type:  application/json

[
  {
    "id": "5A563046-374D-3C4E-4D4A-2CA3812A42C8",
    "name": "puddddtresult",
    "parentId": "D5673FC9-9B1A-7030-C7C5-0B4C7A641EFC"
  },
  ...
]
```

### Get a Public Canned Message Category by id

  `GET /api/v3/globalSettings/publicCannedMessageCategories/{id}`

#### Parameters

Path Parameters

  | Name  | Type | Required  | Description |
  | - | - | - | - |
  | `id` | Guid | yes  |  The id of the public canned message category. |

#### Response

The response is a [Public Canned Message Category](#public-Canned-Message-Category-object) object.

#### Example

Using curl
```
curl -H "Content-Type: application/json"
-X GET https://api1.comm100.io/api/v3/globalSettings/publicCannedMessageCategories/5A563046-374D-3C4E-4D4A-2CA3812A42C8
```

Response
``` json
HTTP/1.1 200 OK
Content-Type:  application/json

{
  "id": "5A563046-374D-3C4E-4D4A-2CA3812A42C8",
  "name": "puddddtresult",
  "parentId": "D5673FC9-9B1A-7030-C7C5-0B4C7A641EFC"
}
```

### Create a new Public Canned Message Category

  `POST /api/v3/globalSettings/publicCannedMessageCategories`

#### Parameters

Request Body

  The request body contains data with the [Public Canned Message Category](#public-Canned-Message-Category-object) structure.

example:
```Json
{
  "name": "testtest",
  "parentId": "D5673FC9-9B1A-7030-C7C5-0B4C7A641EFC"
}
```

#### Response

The response is a [Public Canned Message Category](#public-Canned-Message-Category-object) object.

#### Example

Using curl
```
curl -H "Content-Type: application/json" -d '{
  "name": "testtest",
  "parentId": "D5673FC9-9B1A-7030-C7C5-0B4C7A641EFC"
  }' -X POST https://api1.comm100.io/api/v3/globalSettings/publicCannedMessageCategories
```

Response
```Json
HTTP/1.1 201 Created
Content-Type:  application/json
Location: https://api1.comm100.io/api/v3/globalSettings/publicCannedMessageCategories/7D3E7435-F956-29FE-C089-57241AFBB297

{
  "id": "7D3E7435-F956-29FE-C089-57241AFBB297",
  "name": "testtest",
  "parentId": "D5673FC9-9B1A-7030-C7C5-0B4C7A641EFC"
}
```

### Update a Public Canned Message Category

  `PUT /api/v3/globalSettings/publicCannedMessageCategories/{id}`

#### Parameters

Path Parameters

  | Name  | Type | Required  | Description |
  | - | - | - | - |
  | `id` | Guid | yes  |  The unique Id of the public canned message category. |

Request Body

  The request body contains data with the [Public Canned Message Category](#public-Canned-Message-Category-object) structure.

example:
```Json
{
  "name": "testtest22222",
  "parentId": "D5673FC9-9B1A-7030-C7C5-0B4C7A641EFC"
}
```

#### Response

The response is a [Public Canned Message Category](#public-Canned-Message-Category-object) object.

#### Example

Using curl
```
curl -H "Content-Type: application/json" -d '{
  "name": "testtest22222",
  "parentId": "D5673FC9-9B1A-7030-C7C5-0B4C7A641EFC"
  }' -X PUT https://api1.comm100.io/api/v3/globalSettings/publicCannedMessageCategories/7D3E7435-F956-29FE-C089-57241AFBB297
```

Response
```Json
HTTP/1.1 200 OK
Content-Type:  application/json

{
  "id": "7D3E7435-F956-29FE-C089-57241AFBB297",
  "name": "testtest22222",
  "parentId": "D5673FC9-9B1A-7030-C7C5-0B4C7A641EFC"
}
```

### Delete a Public Canned Message Category

  `DELETE /api/v3/globalSettings/publicCannedMessageCategories/{id}`

#### Parameters

Path Parameters

  | Name  | Type | Required  | Description |
  | - | - | - | - |
  | `id` | Guid | yes  |  The unique Id of the public canned message category.|

#### Response

HTTP/1.1 204 No Content

#### Example

Using curl
```
curl -X DELETE https://api1.comm100.io/api/v3/globalSettings/publicCannedMessageCategories/7D3E7435-F956-29FE-C089-57241AFBB297
```
Response
```json
HTTP/1.1 204 No Content
```

# Public Canned Message

  You need `Manage Public Canned Messages` permission to manage the canned message.

- `GET /api/v3/globalSettings/publicCannedMessages` - [Get all public canned messages in site](#get-all-Public-Canned-Messages-in-site)
- `GET /api/v3/globalSettings/publicCannedMessages/{id}` - [Get a public canned message by id](#get-a-Public-Canned-Message-by-id)
- `POST /api/v3/globalSettings/publicCannedMessages` - [create a new public canned message](#create-a-new-Public-Canned-Message)
- `POST /api/v3/globalSettings/publicCannedMessages/{id}/similarQuestions`  - [create a new public similar questions](#create-a-new-public-similar-questions)
- `PUT /api/v3/globalSettings/publicCannedMessages/{id}` - [update a public canned message](#update-a-Public-Canned-Message)
- `DELETE /api/v3/globalSettings/publicCannedMessages/{id}` - [delete a public canned message](#delete-a-Public-Canned-Message)

## Related Object JSON format

### Public Canned Message Object

  Public Canned Message Object is represented as simple flat JSON objects with the following keys:

  | Name | Type | Include | Read-only | Mandatory | Default | Description |
  | - | - |- | :-: | :-: | :-: | - |
  | `id` | Guid | | yes | no | | Id of the canned message.  |
  | `name` | string | | no | yes | | Name of the canned message. |
  | `message` | string | | no | yes | | |
  | `IfSetHtmlMessageForEmail` | boolean  | | no | no | false | |
  | `htmlMessage` | string  | | no | no | | |
  | `categoryId` | Guid | | no | yes | | |
  | `category` | [Public Canned Message Category](#public-Canned-Message-Category-object)  | yes | no | no | |  Category can be blank. Please note that this is different from Intent Category and Article Category. Available only when `publicCannedMessageCategory` is included. |
  | `shortcuts` | string  | | no | no | | Whether the custom away status is system or not. |
  | `similarQuestions` | string[]  | | no | no | | Available when Agent Assist is enabled. |

## Public Canned Message Endpoints

### Get all Public Canned Messages in site

  `GET /api/v3/globalSettings/publicCannedMessages`

#### Parameters

Query string

  | Name  | Type | Required  | Default | Description |
  | - | - | - | - | - |
  | `include` | string | no  |  | Available value: `publicCannedMessageCategory` |

#### Response

The response is an array of [Public Canned Message](#public-Canned-Message-object) objects.

#### Example

Using curl
```
curl -H "Content-Type: application/json"
-X GET https://api1.comm100.io/api/v3/globalSettings/publicCannedMessages?include=publicCannedMessageCategory
```

Response
``` json
HTTP/1.1 200 OK
Content-Type:  application/json

[
    {
        "id": "C354EA75-BAAF-9994-9307-D001FBE1882A",
        "name": "publicCannedMessageCategory",
        "message": "publicCannedMessageCategory",
        "IfSetHtmlMessageForEmail": false,
        "htmlMessage": "",
        "categoryId": "5A563046-374D-3C4E-4D4A-2CA3812A42C8",
        "category":    { // include public canned message category
          "id": "5A563046-374D-3C4E-4D4A-2CA3812A42C8",
          "name": "puddddtresult",
         "parentId": "D5673FC9-9B1A-7030-C7C5-0B4C7A641EFC"
        },
        "shortcuts": "",
        "similarQuestions": ["are you ok?"]
    },
    ...
]
```

### Get a Public Canned Message by id

  `GET /api/v3/globalSettings/publicCannedMessages/{id}`

#### Parameters

Path Parameters

  | Name  | Type | Required  | Description |
  | - | - | - | - |
  | `id` | Guid | yes  |  The unique Id of the public canned message. |

Query string

  | Name  | Type | Required  | Default | Description |
  | - | - | - | - | - |
  | `include` | string | no  |  | Available value: `publicCannedMessageCategory` |

#### Response

The response is a [Public Canned Message](#public-Canned-Message-object) object.

#### Example

Using curl
```
curl -H "Content-Type: application/json"
-X GET https://api1.comm100.io/api/v3/globalSettings/publicCannedMessages/C354EA75-BAAF-9994-9307-D001FBE1882A?include=publicCannedMessageCategory
```

Response
``` json
HTTP/1.1 200 OK
Content-Type:  application/json

{
  "id": "C354EA75-BAAF-9994-9307-D001FBE1882A",
  "name": "publicCannedMessageCategory",
  "message": "publicCannedMessageCategory",
  "IfSetHtmlMessageForEmail": false,
  "htmlMessage": "",
  "categoryId": "5A563046-374D-3C4E-4D4A-2CA3812A42C8",
  "category":    { // include public canned message category
    "id": "5A563046-374D-3C4E-4D4A-2CA3812A42C8",
    "name": "puddddtresult",
    "parentId": "D5673FC9-9B1A-7030-C7C5-0B4C7A641EFC"
  },
  "shortcuts": "",
  "similarQuestions": ["are you ok?"]
}
```

### Create a new Public Canned Message

  `POST /api/v3/globalSettings/publicCannedMessages`

#### Parameters

Request Body

  The request body contains data with the [Public Canned Message](#public-Canned-Message-object) structure.

example:
```Json
{
  "name": "publicCannedMessageCategorytest",
  "message": "publicCannedMessageCategorytest",
  "IfSetHtmlMessageForEmail": false,
  "htmlMessage": "",
  "categoryId": "5A563046-374D-3C4E-4D4A-2CA3812A42C8",
  "shortcuts": "",
  "similarQuestions": ["are you ok?"]
}
```

#### Response

The response is a [Public Canned Message](#public-Canned-Message-object) object.

#### Example

Using curl
```
curl -H "Content-Type: application/json" -d '{
  "name": "publicCannedMessageCategorytest",
  "message": "publicCannedMessageCategorytest",
  "IfSetHtmlMessageForEmail": false,
  "htmlMessage": "",
  "categoryId": "5A563046-374D-3C4E-4D4A-2CA3812A42C8",
  "shortcuts": "",
  "similarQuestions": ["are you ok?"]
  }' -X POST https://api1.comm100.io/api/v3/globalSettings/publicCannedMessages
```

Response
``` json
HTTP/1.1 201 Created
Content-Type:  application/json
Location: https://api1.comm100.io/api/v3/globalSettings/publicCannedMessages/19B21FEE-B0C5-2A61-0D34-26FB057D15EE

{
  "id": "19B21FEE-B0C5-2A61-0D34-26FB057D15EE",
  "name": "publicCannedMessageCategorytest",
  "message": "publicCannedMessageCategorytest",
  "IfSetHtmlMessageForEmail": false,
  "htmlMessage": "",
  "categoryId": "5A563046-374D-3C4E-4D4A-2CA3812A42C8",
  "shortcuts": "",
  "similarQuestions": ["are you ok?"]
}
```

### Create a new public similar questions

  `POST /api/v3/globalSettings/publicCannedMessages/{id}/similarQuestions`

#### Parameters

Request Body

  The request body contains data with the string array.

example:
```Json
 ["not ok?"]
```

#### Response

The response is a [Public Canned Message](#public-Canned-Message-object) Object.

#### Example

Using curl
```
curl -H "Content-Type: application/json" -d '{
  ["not ok?"]
  }' -X POST https://api1.comm100.io/api/v3/globalSettings/publicCannedMessages/19B21FEE-B0C5-2A61-0D34-26FB057D15EE/similarQuestions
```

Response
``` json
HTTP/1.1 201 Created
Content-Type:  application/json
Location: https://api1.comm100.io/api/v3/globalSettings/publicCannedMessages/19B21FEE-B0C5-2A61-0D34-26FB057D15EE/similarQuestions

{
  "id": "19B21FEE-B0C5-2A61-0D34-26FB057D15EE",
  "name": "publicCannedMessageCategorytest",
  "message": "publicCannedMessageCategorytest",
  "IfSetHtmlMessageForEmail": false,
  "htmlMessage": "",
  "categoryId": "5A563046-374D-3C4E-4D4A-2CA3812A42C8",
  "shortcuts": "",
  "similarQuestions": ["are you ok?","not ok?"]
}
```

### Update a Public Canned Message

  `PUT /api/v3/globalSettings/publicCannedMessages/{id}`

#### Parameters

Path Parameters

  | Name  | Type | Required  | Description |
  | - | - | - | - |
  | `id` | Guid | yes  |  the unique Id of the public canned message |

Request Body

  The request body contains data with the [Public Canned Message](#public-Canned-Message-object) structure.

example:
```Json
{
  "name": "test11111",
  "message": "test11111",
  "IfSetHtmlMessageForEmail": false,
  "htmlMessage": "",
  "categoryId": "5A563046-374D-3C4E-4D4A-2CA3812A42C8",
  "shortcuts": "",
  "similarQuestions": ["are you ok?"]
}
```

#### Response

The response is a [Public Canned Message](#public-Canned-Message-object) Object.

#### Example

Using curl
```
curl -H "Content-Type: application/json" -d '{
  "name": "test11111",
  "message": "test11111",
  "IfSetHtmlMessageForEmail": false,
  "htmlMessage": "",
  "categoryId": "5A563046-374D-3C4E-4D4A-2CA3812A42C8",
  "shortcuts": "",
  "similarQuestions": ["are you ok?"]
  }' -X PUT https://api1.comm100.io/api/v3/globalSettings/publicCannedMessages/19B21FEE-B0C5-2A61-0D34-26FB057D15EE
```

Response
``` json
HTTP/1.1 200 OK
Content-Type:  application/json

{
  "id": "19B21FEE-B0C5-2A61-0D34-26FB057D15EE",
  "name": "test11111",
  "message": "test11111",
  "IfSetHtmlMessageForEmail": false,
  "htmlMessage": "",
  "categoryId": "5A563046-374D-3C4E-4D4A-2CA3812A42C8",
  "shortcuts": "",
  "similarQuestions": ["are you ok?"]
}
```

### Delete a Public Canned Message

  `DELETE /api/v3/globalSettings/publicCannedMessages/{id}`

#### Parameters

Path Parameters

  | Name  | Type | Required  | Description |
  | - | - | - | - |
  | `id` | Guid | yes  |  the unique Id of the public canned message |

#### Response

HTTP/1.1 204 No Content

#### Example

Using curl
```
curl -X DELETE https://api1.comm100.io/api/v3/globalSettings/publicCannedMessages/19B21FEE-B0C5-2A61-0D34-26FB057D15EE
```

Response
```Json
  HTTP/1.1 204 No Content
```

# Private Canned Message Category

You need `Manage Private Canned Messages` permission to manage the canned message category.

- `GET /api/v3/globalSettings/privateCannedMessageCategories` - [Get all private canned message categories in site](#get-all-Private-Canned-Message-Categories-in-site)
- `GET /api/v3/globalSettings/privateCannedMessageCategories/{id}` - [Get a private canned message category by id](#get-a-Private-Canned-Message-Category-by-id)
- `POST /api/v3/globalSettings/privateCannedMessageCategories` - [create a new private canned message category](#create-a-new-Private-Canned-Message-Category)
- `PUT /api/v3/globalSettings/privateCannedMessageCategories/{id}` - [update a private canned message category](#update-a-Private-Canned-Message-Category)
- `DELETE /api/v3/globalSettings/privateCannedMessageCategories/{id}` - [delete a private canned message category](#delete-a-Private-Canned-Message-Category)

## Related Object JSON format

### Private Canned Message Category Object

  Private Canned Message Category Object is represented as simple flat JSON objects with the following keys:

  | Name | Type | Read-only | Mandatory | Default | Description |
  | - | - | :-: | :-: | :-: | - |
  | `id` | Guid | yes | no | | Id of the current item.  |
  | `name` | string  | no | yes | | Name of the canned message category. |
  | `parentId` | Guid  | no | yes | | Parent of the canned message category. |

## Private Canned Message Category Endpoints

### Get all Private Canned Message Categories in site

  `GET /api/v3/globalSettings/privateCannedMessageCategories`

#### Parameters

    No parameters

#### Response

The response is an array of [Private Canned Message Category](#private-Canned-Message-Category-object) objects.

#### Example

Using curl
```
curl -H "Content-Type: application/json"
-X GET https://api1.comm100.io/api/v3/globalSettings/privateCannedMessageCategories
```

Response
``` json
HTTP/1.1 200 OK
Content-Type:  application/json

[
    {
        "id": "119043D0-76A6-D3C1-B594-493111CE1552",
        "name": "tstestsetsteset",
        "parentId": "841193BB-8A51-E4F3-EB17-6B4C222D744F"
    },
    ...
]
```

### Get a Private Canned Message Category by id

  `GET /api/v3/globalSettings/privateCannedMessageCategories/{id}`

#### Parameters

Path Parameters

  | Name  | Type | Required  | Description |
  | - | - | - | - |
  | `id` | Guid | yes  |  the unique Id of the private canned message category |

#### Response

The response is a [Private Canned Message Category](#private-Canned-Message-Category-object) Object.

#### Example

Using curl
```
curl -H "Content-Type: application/json"
-X GET https://api1.comm100.io/api/v3/globalSettings/privateCannedMessageCategories/119043D0-76A6-D3C1-B594-493111CE1552
```

Response
``` json
HTTP/1.1 200 OK
Content-Type:  application/json

{
  "id": "119043D0-76A6-D3C1-B594-493111CE1552",
  "name": "tstestsetsteset",
  "parentId": "841193BB-8A51-E4F3-EB17-6B4C222D744F"
}
```

### Create a new Private Canned Message Category

  `POST /api/v3/globalSettings/privateCannedMessageCategories`

#### Parameters

Request Body

  The request body contains data with the [Private Canned Message Category](#private-Canned-Message-Category-object) structure.

example:
```Json
{
  "name": "testtest111111",
  "parentId": "D5673FC9-9B1A-7030-C7C5-0B4C7A641EFC"
}
```

#### Response

The response is a [Private Canned Message Category](#private-Canned-Message-Category-object) Object.

#### Example

Using curl
```
curl -H "Content-Type: application/json" -d '{
  "name": "testtest111111",
  "parentId": "841193BB-8A51-E4F3-EB17-6B4C222D744F"
  }' -X POST https://api1.comm100.io/api/v3/globalSettings/privateCannedMessageCategories
```

Response
```Json
HTTP/1.1 201 Created
Content-Type:  application/json
Location: https://api1.comm100.io/api/v3/globalSettings/privateCannedMessageCategories/FFD377AA-81FA-EC53-1E57-DD73C0B36F6C
{
  "id": "FFD377AA-81FA-EC53-1E57-DD73C0B36F6C",
  "name": "testtest111111",
  "parentId": "841193BB-8A51-E4F3-EB17-6B4C222D744F"
}
```

### Update a Private Canned Message Category

  `PUT /api/v3/globalSettings/privateCannedMessageCategories/{id}`

#### Parameters

Path Parameters

  | Name  | Type | Required  | Description |
  | - | - | - | - |
  | `id` | Guid | yes  |  the unique Id of the private canned message category |

Request Body

  The request body contains data with the [Private Canned Message Category](#private-Canned-Message-Category-object) structure.

example:
```Json
{
  "name": "testtest22222",
  "parentId": "841193BB-8A51-E4F3-EB17-6B4C222D744F"
}
```

#### Response

The response is a [Private Canned Message Category](#private-Canned-Message-Category-object) Object.

#### Example

Using curl
```
curl -H "Content-Type: application/json" -d '{
  "name": "testtest22222",
  "parentId": "D5673FC9-9B1A-7030-C7C5-0B4C7A641EFC"
  }' -X PUT https://api1.comm100.io/api/v3/globalSettings/privateCannedMessageCategories/FFD377AA-81FA-EC53-1E57-DD73C0B36F6C
```

Response
```Json
  HTTP/1.1 200 OK
  Content-Type:  application/json

{
  "id": "FFD377AA-81FA-EC53-1E57-DD73C0B36F6C",
  "name": "testtest22222",
  "parentId": "841193BB-8A51-E4F3-EB17-6B4C222D744F"
}
```

### Delete a Private Canned Message Category

  `DELETE /api/v3/globalSettings/privateCannedMessageCategories/{id}`

#### Example

Using curl
```
curl -X DELETE https://api1.comm100.io/api/v3/globalSettings/privateCannedMessageCategories/FFD377AA-81FA-EC53-1E57-DD73C0B36F6C
```

Response
```Json
  HTTP/1.1 204 No Content
```

#### Parameters

Path Parameters

  | Name  | Type | Required  | Description |
  | - | - | - | - |
  | `id` | Guid | yes  |  the unique Id of the private canned message category |

#### Response

HTTP/1.1 204 No Content

# Private Canned Message

You need `Manage Private Canned Messages` permission to manage the canned message.

- `GET /api/v3/globalSettings/privateCannedMessages` - [Get all private canned messages in site](#get-all-Private-Canned-Messages-in-site)
- `GET /api/v3/globalSettings/privateCannedMessages/{id}` - [Get a private canned message by id](#get-a-Private-Canned-Message-by-id)
- `POST /api/v3/globalSettings/privateCannedMessages` - [create a new private canned message](#create-a-new-Private-Canned-Message)
- `POST /api/v3/globalSettings/publicCannedMessages/{id}/similarQuestions`  - [create a new private similar questions](#create-a-new-private-similar-questions)
- `PUT /api/v3/globalSettings/privateCannedMessages/{id}` - [update a private canned message](#update-a-Private-Canned-Message)
- `DELETE /api/v3/globalSettings/privateCannedMessages/{id}` - [delete a private canned message](#delete-a-Private-Canned-Message)

## Related Object JSON format

### Private Canned Message Object

  Private Canned Message Object is represented as simple flat JSON objects with the following keys:

  | Name | Type | Include | Read-only | Mandatory | Default | Description |
  | - | - |- | :-: | :-: | :-: | - |
  | `id` | Guid | | yes | no | | Id of the current item.  |
  | `name` | string  | | no | no | | Name of the canned message. |
  | `message` | string  | | no | no | | |
  | `IfSetHtmlMessageForEmail` | bool | | no | no | false | |
  | `htmlMessage` | string  | | no | no | | |
  | `categoryId` | Guid | | no | no | | |
  | `category` | [Private Canned Message Category](#private-Canned-Message-Category-object)  | yes | no | no | |  Category can be blank. Please note that this is different from Intent Category and Article Category. Available only when `privateCannedMessageCategory` is included. |
  | `shortcuts` | string  | | no | no | | Whether the custom away status is system or not. |
  | `similarQuestions` | string[]  | | no | no | | Available when Agent Assist is enabled. |


## Private Canned Message Endpoints

### Get all Private Canned Messages in site

  `GET /api/v3/globalSettings/privateCannedMessages`

#### Parameters

Query string

  | Name  | Type | Required  | Default | Description |
  | - | - | - | - | - |
  | `include` | string | no  |  | Available value: `privateCannedMessageCategory` |

#### Response

The response is a list of [Private Canned Message](#private-Canned-Message-object) Object.

#### Example

Using curl
```
curl -H "Content-Type: application/json"
-X GET https://api1.comm100.io/api/v3/globalSettings/privateCannedMessages?include=privateCannedMessageCategory
```

Response
``` json
HTTP/1.1 200 OK
Content-Type:  application/json

[
    {
        "id": "AB71E95A-1FA5-E19D-9BA4-9C2144598C57",
        "name": "privateCannedMessageCategory",
        "message": "privateCannedMessageCategory",
        "IfSetHtmlMessageForEmail": false,
        "htmlMessage": "",
        "categoryId": "579BCAE9-F43A-CD32-CAF8-BD56786F1447",
        "category":    { // include private canned message category
          "id": "579BCAE9-F43A-CD32-CAF8-BD56786F1447",
          "name": "justfortest",
         "parentId": "A73FA2EB-4CE3-B195-94C6-567A24F7BDDC"
        },
        "shortcuts": "",
        "similarQuestions": ["are you ok?"]
    },
    ...
]
```

### Get a Private Canned Message by id

  `GET /api/v3/globalSettings/privateCannedMessages/{id}`

#### Parameters

Path Parameters

  | Name  | Type | Required  | Description |
  | - | - | - | - |
  | `id` | Guid | yes  |  the unique Id of the private canned message |

Query string

  | Name  | Type | Required  | Default | Description |
  | - | - | - | - | - |
  | `include` | string | no  |  | Available value: `privateCannedMessageCategory` |

##### Response

The response is a [Private Canned Message](#private-Canned-Message-object) Object.

#### Example

Using curl
```
curl -H "Content-Type: application/json"
-X GET https://api1.comm100.io/api/v3/globalSettings/privateCannedMessages/AB71E95A-1FA5-E19D-9BA4-9C2144598C57?include=privateCannedMessageCategory
```

Response
``` json
HTTP/1.1 200 OK
Content-Type:  application/json

{
  "id": "C354EA75-BAAF-9994-9307-D001FBE1882A",
  "name": "privateCannedMessageCategory",
  "message": "privateCannedMessageCategory",
  "IfSetHtmlMessageForEmail": false,
  "htmlMessage": "",
  "categoryId": "579BCAE9-F43A-CD32-CAF8-BD56786F1447",
  "category":    { // include private canned message category
    "id": "5A563046-374D-3C4E-4D4A-2CA3812A42C8",
    "name": "puddddtresult",
    "parentId": "A73FA2EB-4CE3-B195-94C6-567A24F7BDDC"
  },
  "shortcuts": "",
  "similarQuestions": ["are you ok?"]
}
```

### Create a new Private Canned Message

  `POST /api/v3/globalSettings/privateCannedMessages`

#### Parameters

Request Body

  The request body contains data with the [Private Canned Message](#private-Canned-Message-object) structure.

example:
```Json
{
  "name": "privateCannedMessageCategorytest1111",
  "message": "privateCannedMessageCategorytest1111",
  "IfSetHtmlMessageForEmail": false,
  "htmlMessage": "",
  "categoryId": "579BCAE9-F43A-CD32-CAF8-BD56786F1447",
  "shortcuts": "",
  "similarQuestions": ["are you ok?"]
}
```

#### Response

The response is a [Private Canned Message](#private-Canned-Message-object) Object.

#### Example

Using curl
```
curl -H "Content-Type: application/json" -d '{
  "name": "privateCannedMessageCategorytest1111",
  "message": "privateCannedMessageCategorytest1111",
  "IfSetHtmlMessageForEmail": false,
  "htmlMessage": "",
  "categoryId": "579BCAE9-F43A-CD32-CAF8-BD56786F1447",
  "shortcuts": "",
  "similarQuestions": ["are you ok?"]
  }' -X POST https://api1.comm100.io/api/v3/globalSettings/privateCannedMessages
```

Response
``` json
HTTP/1.1 201 Created
Content-Type:  application/json
Location: https://api1.comm100.io/api/v3/globalSettings/privateCannedMessages/822B7B6A-05E9-5DA2-A1B0-1D0FB034AA0F

{
  "id": "822B7B6A-05E9-5DA2-A1B0-1D0FB034AA0F",
  "name": "privateCannedMessageCategorytest1111",
  "message": "privateCannedMessageCategorytest1111",
  "IfSetHtmlMessageForEmail": false,
  "htmlMessage": "",
  "categoryId": "579BCAE9-F43A-CD32-CAF8-BD56786F1447",
  "shortcuts": "",
  "similarQuestions": ["are you ok?"]
}
```

### Create a new private similar questions

  `POST /api/v3/globalSettings/privateCannedMessages/{id}/similarQuestions`

#### Parameters

Request Body

  The request body contains data with the string array.

example:
```Json
 ["not ok?"]
```

#### Response

The response is a [Public Canned Message](#public-Canned-Message-object) Object.

#### Example

Using curl
```
curl -H "Content-Type: application/json" -d '{
  ["not ok?"]
  }' -X POST https://api1.comm100.io/api/v3/globalSettings/privateCannedMessages/822B7B6A-05E9-5DA2-A1B0-1D0FB034AA0F/similarQuestions
```

Response
``` json
HTTP/1.1 201 Created
Content-Type:  application/json
Location: https://api1.comm100.io/api/v3/globalSettings/privateCannedMessages/822B7B6A-05E9-5DA2-A1B0-1D0FB034AA0F/similarQuestions

{
  "id": "822B7B6A-05E9-5DA2-A1B0-1D0FB034AA0F",
  "name": "publicCannedMessageCategorytest",
  "message": "publicCannedMessageCategorytest",
  "IfSetHtmlMessageForEmail": false,
  "htmlMessage": "",
  "categoryId": "5A563046-374D-3C4E-4D4A-2CA3812A42C8",
  "shortcuts": "",
  "similarQuestions": ["are you ok?","not ok?"]
}
```
### Update a Private Canned Message

  `PUT /api/v3/globalSettings/privateCannedMessages/{id}`

#### Parameters

Path Parameters

  | Name  | Type | Required  | Description |
  | - | - | - | - |
  | `id` | Guid | yes  |  the unique Id of the private canned message |

Request Body

  The request body contains data with the [Private Canned Message](#private-Canned-Message-object) structure.

example:
```Json
{
  "name": "privateCannedMessageCategorytest2222",
  "message": "privateCannedMessageCategorytest2222",
  "IfSetHtmlMessageForEmail": false,
  "htmlMessage": "",
  "categoryId": "579BCAE9-F43A-CD32-CAF8-BD56786F1447",
  "shortcuts": "",
  "similarQuestions": ["are you ok?"]
}
```

#### Response

The response is a [Private Canned Message](#private-Canned-Message-object) Object.

#### Example

Using curl
```
curl -H "Content-Type: application/json" -d '{
  "name": "privateCannedMessageCategorytest2222",
  "message": "privateCannedMessageCategorytest2222",
  "IfSetHtmlMessageForEmail": false,
  "htmlMessage": "",
  "categoryId": "579BCAE9-F43A-CD32-CAF8-BD56786F1447",
  "shortcuts": "",
  "similarQuestions": ["are you ok?"]
  }' -X PUT https://api1.comm100.io/api/v3/globalSettings/privateCannedMessages/822B7B6A-05E9-5DA2-A1B0-1D0FB034AA0F
```

Response
``` json
HTTP/1.1 200 OK
Content-Type:  application/json

{
  "id": "822B7B6A-05E9-5DA2-A1B0-1D0FB034AA0F",
  "name": "privateCannedMessageCategorytest2222",
  "message": "privateCannedMessageCategorytest2222",
  "IfSetHtmlMessageForEmail": false,
  "htmlMessage": "",
  "categoryId": "579BCAE9-F43A-CD32-CAF8-BD56786F1447",
  "shortcuts": "",
  "similarQuestions": ["are you ok?"]
}
```

### Delete a Private Canned Message

  `DELETE /api/v3/globalSettings/privateCannedMessages/{id}`

#### Parameters

Path Parameters

  | Name  | Type | Required  | Description |
  | - | - | - | - |
  | `id` | Guid | yes  |  the unique Id of the private canned message |

#### Response

HTTP/1.1 204 No Content

#### Example

Using curl
```
curl -X DELETE https://api1.comm100.io/api/v3/globalSettings/privateCannedMessages/822B7B6A-05E9-5DA2-A1B0-1D0FB034AA0F
```

Response
```Json
  HTTP/1.1 204 No Content
```


# Agent Away Status

  You need `Manage Custom Away Status` permission to manage `Agent Away Status`.

- `GET /api/v3/globalSettings/agentAwayStatuses` - [Get all agent away statuses in site](#get-all-Agent-Away-Statuses-in-site)
- `GET /api/v3/globalSettings/agentAwayStatuses/{id}` - [Get an agent away status by id](#get-an-Agent-Away-Status-by-id)
- `POST /api/v3/globalSettings/agentAwayStatuses` - [create a new agent away status](#create-a-new-Agent-Away-Status)
- `PUT /api/v3/globalSettings/agentAwayStatuses/{id}` - [update an agent away status](#update-an-Agent-Away-Status)
- `DELETE /api/v3/globalSettings/agentAwayStatuses/{id}` - [delete an agent away status](#delete-an-Agent-Away-Status)

## Related Object JSON format

### Agent Away Status Object

  Agent Away Status Object is represented as simple flat JSON objects with the following keys:

  | Name | Type | Read-only | Mandatory| Default | Description |
  | - | - | :-: | :-: | :-: | - |
  | `id` | Guid | yes | no | | Id of the current item.  |
  | `name` | string  | no | yes | | Name of the agent away status. |
  | `isSystem` | boolean  | yes | no | false | Whether the agent away status is system or not. |
  | `order` | integer  | yes | no | | The order of the agent away status. |

## Agent Away Status Endpoints

### Get all Agent Away Statuses of a site

  `GET /api/v3/globalSettings/agentAwayStatuses`

#### Parameters

    No parameters

#### Response

The response is an array of [Agent Away Status](#agent-Away-Status-object) objects.

#### Example

Using curl
```
curl -H "Content-Type: application/json"
-X GET https://api1.comm100.io/api/v3/globalSettings/agentAwayStatuses
```

Response
``` json
HTTP/1.1 200 OK
Content-Type:  application/json

[
    {
        "id": "BAACB779-2E41-27C5-B23D-1C8F2058862D",
        "name": "agentAwayStatuses",
        "isSystem": false,
        "order": 1
    },
    ...
]
```

### Get an Agent Away Status by id

  `GET /api/v3/globalSettings/agentAwayStatuses/{id}`

#### Parameters

Path Parameters

  | Name  | Type | Required  | Description |
  | - | - | - | - |
  | `id` | Guid | yes  |  The unique Id of the agent away status. |

#### Response

The response is an [Agent Away Status](#agent-Away-Status-object) object.

#### Example

Using curl
```
curl -H "Content-Type: application/json"
-X GET https://api1.comm100.io/api/v3/globalSettings/agentAwayStatuses/BAACB779-2E41-27C5-B23D-1C8F2058862D
```

Response
``` json
HTTP/1.1 200 OK
Content-Type:  application/json

{
  "id": "BAACB779-2E41-27C5-B23D-1C8F2058862D",
  "name": "agentAwayStatuses",
  "isSystem": false,
  "order": 1
}
```

### Create a new Agent Away Status

  `POST /api/v3/globalSettings/agentAwayStatuses`

#### Parameters

Request Body

  The request body contains data with the [Agent Away Status](#agent-Away-Status-object) structure.

example:
```Json
{
  "name": "agentAwayStatuses11",
  "isSystem": false,
  "order": 1
}
```

#### Response

The response is an [Agent Away Status](#agent-Away-Status-object) object.

#### Example

Using curl
```
curl -H "Content-Type: application/json" -d '{
  "name": "agentAwayStatuses11"
  }' -X POST https://api1.comm100.io/api/v3/globalSettings/agentAwayStatuses
```

Response
``` json
HTTP/1.1 201 Created
Content-Type:  application/json
Location: https://api1.comm100.io/api/v3/globalSettings/agentAwayStatuses/D4F6BA7F-9BB6-C509-8BB9-0705B3E500F2

{
  "id": "D4F6BA7F-9BB6-C509-8BB9-0705B3E500F2",
  "name": "agentAwayStatuses11",
  "isSystem": false,
  "order": 5
}
```

### Update an Agent Away Status

  `PUT /api/v3/globalSettings/agentAwayStatuses/{id}`

#### Parameters

Path Parameters

  | Name  | Type | Required  | Description |
  | - | - | - | - |
  | `id` | Guid | yes  |  The unique Id of the agent away status.|

Request Body

  The request body contains data with the [Agent Away Status](#agent-Away-Status-object) structure.

example:
```Json
{
  "name": "agentAwayStatuses23",
}
```

#### Response

The response is an [Agent Away Status](#agent-Away-Status-object) object.

#### Example

Using curl
```
curl -H "Content-Type: application/json" -d '{
  "name": "agentAwayStatuses22",
  "isSystem": false,
  "order": 1
  }' -X PUT https://api1.comm100.io/api/v3/globalSettings/agentAwayStatuses/D4F6BA7F-9BB6-C509-8BB9-0705B3E500F2
```

Response
``` json
HTTP/1.1 200 OK
Content-Type:  application/json

{
  "id": "D4F6BA7F-9BB6-C509-8BB9-0705B3E500F2",
  "name": "agentAwayStatuses23",
  "isSystem": false,
  "order": 5
}
```

### Delete an Agent Away Status

  `DELETE /api/v3/globalSettings/agentAwayStatuses/{id}`

#### Parameters

Path Parameters

  | Name  | Type | Required  | Description |
  | - | - | - | - |
  | `id` | Guid | yes  |  The unique Id of the agent away status.|

#### Response

HTTP/1.1 204 No Content

#### Example

Using curl
```
curl -X DELETE https://api1.comm100.io/api/v3/globalSettings/agentAwayStatuses/D4F6BA7F-9BB6-C509-8BB9-0705B3E500F2
```

Response
```Json
  HTTP/1.1 204 No Content
```

# Whitelisted Login IP Range

You need `Manage Security` permission to manage whitelisted login ip restrictions.

   When Login IP Whitelist is enabled, site administrator can add IP range for this site and only agents within the IP range can log in successfully.

  + `GET /api/v3/globalSettings/whitelistedLoginIPRanges` - [Get a list of whitelisted login IP ranges in site](#get-all-whitelisted-login-ip-ranges)
  + `GET /api/v3/globalSettings/whitelistedLoginIPRanges/{id}` - [Get a whitelisted login IP range by id](#get-a-whitelisted-login-ip-ranges)
  + `POST /api/v3/globalSettings/whitelistedLoginIPRanges` - [create a new whitelisted login IP range](#create-a-new-whitelisted-login-ip-range)
  + `PUT /api/v3/globalSettings/whitelistedLoginIPRanges/{id}` - [update a whitelisted login IP range](#update-a-whitelisted-login-ip-range)
  + `DELETE /api/v3/globalSettings/whitelistedLoginIPRanges/{id}` - [delete a whitelisted login IP range](#delete-a-whitelisted-login-ip-range)

## Whitelisted Login IP Range Related Object JSON format

### Whitelisted Login IP Range Object

  | Name | Type | Include | Read-only | Mandatory | Default | Description |
  | - | - | :-: | :-: | :-: | :-: | - |
  |`id` | Guid | | yes | no | | |
  |`ipFrom` | string | | no | yes | | Where an IP range starts.|
  |`ipTo` | string | | no | yes | | Where an IP range ends.|
  |`createdTime` | DateTime | | no | no |  |  |

## Whitelisted Login IP Range Endpoints

### Get all Whitelisted Login IP Ranges

  `GET /api/v3/globalSettings/whitelistedLoginIPRanges`

  #### Parameters
    No parameters

  #### Response

  The response is a list of [Whitelisted Login IP Range](#whitelisted-login-ip-range-object) Objects

#### Example
Using curl
```
curl -H "Content-Type: application/json" -X GET https://api1.comm100.io/api/v3/globalSettings/whitelistedLoginIPRanges
```
Response
```json
HTTP/1.1 200 OK
Content-Type:  application/json[
{
  "id": "42dwdaww-92e6-4487-a2e8-92e68d6892e6",
  "ipFrom": "201.195.21.5",
  "ipTo": "201.195.21.8",
  "createdTime": "2020-01-01",
},
...,
]
```

### Get a Whitelisted Login IP Ranges

  `GET /api/v3/globalSettings/whitelistedLoginIPRanges/{id}`

#### Parameters
Path parameters

  | Name  | Type | Required  | Description |
  | - | - | - | - |
  |`id` | Guid | yes  |  The id of the whitelisted Login IP Ranges |

#### Response

The response is a [Whitelisted Login IP Range](#whitelisted-login-ip-range-object) Object.

#### Example
Using curl
```
curl -H "Content-Type: application/json" -X GET https://api1.comm100.io/api/v3/globalSettings/whitelistedLoginIPRanges/42dwdaww-92e6-4487-a2e8-92e68d6892e6
```
Response
```json
HTTP/1.1 200 OK
Content-Type:  application/json
  {
  "id": "42dwdaww-92e6-4487-a2e8-92e68d6892e6",
  "ipFrom": "201.195.21.5",
  "ipTo": "201.195.21.8",
  "createdTime": "2020-01-01",
  }
```

### Create a new whitelisted Login IP Range

  `POST /api/v3/globalSettings/whitelistedLoginIPRanges`

####  Parameters

Request body
  The request body contains data with the [Whitelisted Login IP Range](#whitelisted-login-ip-range-object) structure.

  example:
```json
 {
    "ipFrom": "201.195.21.5",
    "ipTo": "201.195.21.8",
  }
```

#### Response

The response is a [Whitelisted Login IP Range](#whitelisted-login-ip-range-object) Object.

#### Example

Using curl
```
curl -H "Content-Type: application/json" -d ' {
      "ipFrom": "201.195.21.5",
      "ipTo": "201.195.21.8",
    }' -X POST https://api1.comm100.io/api/v3/globalSettings/whitelistedLoginIPRanges
```
Response
```json
HTTP/1.1 201 Created
Content-Type:  application/json
Location: https://api1.comm100.io/api/v3/globalSettings/whitelistedLoginIPRanges/42dwdaww-92e6-4487-a2e8-92e68d6892e6
 {
    "id": "42dwdaww-92e6-4487-a2e8-92e68d6892e6",
    "ipFrom": "201.195.21.5",
    "ipTo": "201.195.21.8",
    "createdTime": "2020-01-01",
  }
```


### Update a whitelisted Login IP Range
  `PUT /api/v3/globalSettings/whitelistedLoginIPRanges/{id}`

####  Parameters

Path parameters

  | Name  | Type | Required  | Description |
  | - | - | - | - |
  |`id` | Guid | yes  |  The id of the whitelisted Login IP Range |

Request body

  The request body contains data with the [Whitelisted Login IP Range](#whitelisted-login-ip-range-object) structure.

  example:
```json
 {
    "ipFrom": "201.195.21.5",
    "ipTo": "201.195.21.8",
  }
```

#### Response

The response is a [Whitelisted Login IP Range](#whitelisted-login-ip-range-object) Object.

#### Example
Using curl
```
curl -H "Content-Type: application/json" -d ' {
      "ipFrom": "201.195.21.5",
      "ipTo": "201.195.21.8",
    }' -X PUT https://api1.comm100.io/api/v3/globalSettings/whitelistedLoginIPRanges/42dwdaww-92e6-4487-a2e8-92e68d6892e6
```
Response
```json
  HTTP/1.1 200 OK
  Content-Type: https://api1.comm100.io/api/v3/globalSettings/whitelistedLoginIPRanges/42dwdaww-92e6-4487-a2e8-92e68d6892e6
 {
    "id": "42dwdaww-92e6-4487-a2e8-92e68d6892e6",
    "ipFrom": "201.195.21.5",
    "ipTo": "201.195.21.8",
    "createdTime": "2020-01-01",
  }
```

### Delete a whitelisted Login IP Range

  `DELETE /api/v3/globalSettings/whitelistedLoginIPRanges/{id}`

#### Parameters

Path parameters

  | Name  | Type | Required  | Description |
  | - | - | - | - |
  |`id` | Guid | yes  |  the whitelisted Login IP Range id |

#### Response
HTTP/1.1 204 No Content

#### Example
Using curl
```
curl -X DELETE https://api1.comm100.io/api/v3/globalSettings/whitelistedLoginIPRanges/4487fc9d-92e6-4487-a2e8-92e68d6892e6
```
Response
```json
HTTP/1.1 204 No Content
```

# Agent SSO

  You need `Manage Security` permission to set sso for a site.

- `GET /api/v3/globalSettings/agentSSO` - [Get Agent SSO](#get-agent-sso)
- `PUT /api/v3/globalSettings/agentSSO` - [update Agent SSO](#update-agent-sso)

## Related Object JSON format

### Agent SSO Object

  `Agent SSO` Object is represented as simple flat JSON objects with the following keys:

  | Name | Type | Include | Read-only| Mandatory| Default | Description |
  | - | - | - | :-: | :-: | :-: | - |
  | `isEnabled` | bool  | | no | no|false| |
  | `protocolType` | string |  | no | yes | | Including `SAML` and `JWT`. |
  | `samlSSOURL` | string |  | no |yes | |Mandatory when type is `SAML`. |
  | `samlLogoutURL` | string |  | no | no | | Only available when type is `SAML`. |
  | `samlCertificate` | string |  | no | yes | | SAML certificate, mandatory when type is `SAML`.|
  | `jwtLoginURL` | string |  | no | yes | | Mandatory when type is `JWT`. |
  | `jwtLogoutURL` | string |  | no | no | | Only available when type is `JWT`.  |
  | `jwtSecret` | string |  | no | no | | Mandatory when type is `JWT`.  |

## Agent SSO Endpoints

### Get Agent SSO

  `GET /api/v3/globalSettings/agentSSO`

#### Parameters

    No parameters

#### Response

The response is an [Agent SSO](#agent-SSO-object) object.

#### Example

Using curl
```
curl -H "Content-Type: application/json"
-X GET https://api1.comm100.io/api/v3/globalSettings/agentSSO
```
Response
``` json
HTTP/1.1 200 OK
Content-Type:  application/json

{
  "isEnabled": true,
  "protocolType": "SAML",
  "samlSSOURL": "https://api1.comm100.io/SAML/SSOLogin",
  "samlLogoutURL": "https://api1.comm100.io/SAML/SSOLogout",
  "samlCertificate": "9F4709DB-C391-4896-94BA-3A17BE12D9E2jji-----",
  "jwtLoginURL": "",
  "jwtLogoutURL": "",
  "jwtSecret": ""
}
```

### Update Agent SSO

  `PUT /api/v3/globalSettings/agentSSO`

#### Parameters

Request Body

  The request body contains data with the [Agent SSO](#agent-SSO-object) structure.

 example:
```Json
  {
    "isEnabled": true,
    "protocolType": "JWT",
    "jwtLoginURL": "https://api1.comm100.io/JWT/SSOLogin",
    "jwtLogoutURL": "https://api1.comm100.io/JWT/SSOLogout",
    "jwtSecret": "9F4709DB-C391-4896-94BA-3A17BE12D9E2jji-----"
  }
```

#### Response

The response is an [Agent SSO](#agent-SSO-object) object.

#### Example
Using curl
```
curl -H "Content-Type: application/json" -d '{
    "isEnabled": true,
    "protocolType": "JWT",
    "jwtLoginURL": "https://api1.comm100.io/JWT/SSOLogin",
    "jwtLogoutURL": "https://api1.comm100.io/JWT/SSOLogout",
    "jwtSecret": "9F4709DB-C391-4896-94BA-3A17BE12D9E2jji-----"
  }' -X PUT https://api1.comm100.io/api/v3/globalSettings/agentSSO
```
Response
```Json
HTTP/1.1 200 OK
Content-Type:  application/json

{
  "isEnabled": true,
  "protocolType": "JWT",
  "samlSSOURL": "",
  "samlLogoutURL": "",
  "samlCertificate": "",
  "jwtLoginURL": "https://api1.comm100.io/JWT/SSOLogin",
  "jwtLogoutURL": "https://api1.comm100.io/JWT/SSOLogout",
  "jwtSecret": "9F4709DB-C391-4896-94BA-3A17BE12D9E2jji-----"
}
```

# Visitor SSO

  You need `Manage Settings` permission to set sso for a site.

- `GET /api/v3/globalSettings/visitorSSO` - [Get Visitor SSO](#get-visitor-sso)
- `PUT /api/v3/globalSettings/visitorSSO` - [update Visitor SSO](#update-visitor-sso)

## Related Object JSON format

### Visitor SSO Object

  Visitor SSO Object is represented as simple flat JSON objects with the following keys:

  | Name | Type | Include | Read-only | Mandatory | Default | Description |
  | - | - |- | :-: | :-: | :-: | - |
  | `isEnabled` | bool  | | no | no | false | |
  | `loginURL` | string  | | no | yes | | |
  | `certificate` | string  | | no | yes | | Base64 data of certificate file. |
  | `certificateFileName` | string  | | no | yes | | |
  | `fieldMappings` | [Field Mapping](#field-Mapping-object)[]  | | no | no | | |
  | `perCampaign` | [Visitor SSO Campaign](#visitor-SSO-Campaign-object)[]  |  | no | no | | |

### Field Mapping Object

Field Mapping is represented as simple flat JSON objects with the following keys:

  | Name | Type | Include | Read-only | Mandatory | Default | Description |
  | - | - |- | :-: | :-: | :-: | - |
  | `attribute` | string | | no | yes | | SSO attribute name. |
  | `comm100Field` | string | | no | yes | | The Comm100 field name.|

### Visitor SSO Campaign Object

Visitor SSO Campaign is represented as simple flat JSON objects with the following keys:

  | Name | Type | Include | Read-only | Mandatory | Default | Description |
  | - | - |- | :-: | :-: | :-: | - |
  | `campaignId` | Guid |  | no | yes | | Id of the campaign. |
  | `campaign` | [Campaign](./Livechat%20API.md#campaign-object)  | yes | no | no | | Available only when campaign is included  |
  | `signInOption` | string |  | no | no | `noSignIn` | Type of the sign in, including `noSignIn`, `signInOptional` and `signInRequired`. |
  | `isPrechatFromSkipped` | bool |  | no | no | true | Whether the pre-chat form is skipped when visitors sign in. |

## Visitor SSO Endpoints

### Get Visitor SSO

  `GET /api/v3/globalSettings/visitorSSO`

#### Parameters

Query string

  | Name  | Type | Required | Default | Description |
  | - | - | :-: | :-: | - |
  | `include` | string | no  | |  Available value: `campaign` |

#### Response

The response is a [Visitor SSO](#visitor-SSO-object) object.

#### Example

Using curl
```
curl -H "Content-Type: application/json"
-X GET https://api1.comm100.io/api/v3/globalSettings/visitorSSO?include=campaign
```
Response
``` json
HTTP/1.1 200 OK
Content-Type:  application/json

{
    "isEnabled": true,
    "loginURL": "http://www.xzcs11ffffffff1a",
    "certificate": "amdoamdoaGdmcnRk",
    "certificateFileName": "r.txt",
    "fieldMappings": [
        {
            "attribute": "test999",
            "comm100Field": "{!Visitor.Name}"
        },
        {
            "attribute": "test666",
            "comm100Field": "{!Prechat.Company}"
        }
    ],
    "perCampaign": [
        {
            "campaignId": "cdab2038-1d6c-4fa0-bbf0-17be2e5b39ec",
            "campaign": { //include campaign
                "id":"cdab2038-1d6c-4fa0-bbf0-17be2e5b39ec",
                "name":"testCampaign",
                ...
            },
            "signInOption": "noSignIn",
            "isPrechatFromSkipped": false
        }
    ]
}
```

### Update Visitor SSO

  `PUT /api/v3/globalSettings/visitorSSO`

#### Parameters

Request Body

  The request body contains data with the [Visitor SSO](#visitor-SSO-object) structure.

example:
```Json
  {
    "isEnabled": true,
    "loginURL": "http://www.xzcs11ffffffff1a",
    "certificate": "amdoamdoaGdmcnRk",
    "certificateFileName": "r.txt",
    "fieldMappings": [
        {
            "attribute": "test999",
            "comm100Field": "{!Visitor.Name}"
        },
        {
            "attribute": "test666",
            "comm100Field": "{!Prechat.Company}"
        }
    ],
    "perCampaign": [
        {
            "campaignId": "cdab2038-1d6c-4fa0-bbf0-17be2e5b39ec",
            "signInOption": "noSignIn",
            "isPrechatFromSkipped": false
        }
    ]
  }
```

#### Response

The response is a [Visitor SSO](#visitor-SSO-object) object.

#### Example
Using curl
```
curl -H "Content-Type: application/json" -d '{
    "isEnabled": true,
    "loginURL": "http://www.xzcs11ffffffff1a",
    "certificate": "amdoamdoaGdmcnRk",
    "certificateFileName": "r.txt",
    "fieldMappings": [
        {
            "attribute": "test999",
            "comm100Field": "{!Visitor.Name}"
        },
        {
            "attribute": "test666",
            "comm100Field": "{!Prechat.Company}"
        }
    ],
    "perCampaign": [
        {
            "campaignId": "cdab2038-1d6c-4fa0-bbf0-17be2e5b39ec",
            "signInOption": "noSignIn",
            "isPrechatFromSkipped": false
        }
    ]
  }' -X PUT https://api1.comm100.io/api/v3/globalSettings/visitorSSO
```
Response
```Json
  HTTP/1.1 200 OK
  Content-Type:  application/json

  {
    "isEnabled": true,
    "loginURL": "http://www.xzcs11ffffffff1a",
    "certificate": "amdoamdoaGdmcnRk",
    "certificateFileName": "r.txt",
    "fieldMappings": [
        {
            "attribute": "test999",
            "comm100Field": "{!Visitor.Name}"
        },
        {
            "attribute": "test666",
            "comm100Field": "{!Prechat.Company}"
        }
    ],
    "perCampaign": [
        {
            "campaignId": "cdab2038-1d6c-4fa0-bbf0-17be2e5b39ec",
            "signInOption": "noSignIn",
            "isPrechatFromSkipped": false
        }
    ]
  }
```

# Audit Log
  You need `View Audit Log` permission to view audit logs.

  + `GET /api/v3/globalSettings/auditLogs` - [Get audit logs list](#get-audit-logs-list)

## Audit Log Object JSON format

### Audit Log Object

  | Name | Type | Include | Read-only | Mandatory | Default | Description |
  | - | - | :-: | :-: | :-: | :-: | - |
  |`id` | integer | | yes | no | |  |
  |`category` | string| | no | no | | The value options include: liveChat, ticketingAndMessaging, bot, globalSettings, knowledgeBase |
  |`actionTime` | DateTime | | no | no |  |  |
  |`actionType` | string | | no  | no  | | [action types for different applications](#action-types-for-different-applications) |
  |`actionSummary` | string| | no  | no  | |  |
  |`actionDetails` | string| | no  | no  | |  |
  |`createdBy` | integer | | no  | no  | | the id of oprator agent |
  |`agent` | [Agent](#agent) | yes | no  | no  | | the oprator agent |

### Audit Log List Response Object

  Agent List Object for agent list Response, include count and page information.

  | Name | Type | Include | Read-only | Mandatory | Default | Description |
  | - | - | :-: | :-: | :-: | :-: | - |
  |`count` | integer  | no | yes | no | | The total count of the query  |
  |`nextPage` | string  | no | yes | no | | The next page url of the query  |
  |`previousPage` | string  | no | yes | no | | The previous page url of the query  |
  |`auditLogs`| [AuditLog](#audit-log-object)[]| no | yes| 0 | | a list of Audit Log. |

### Action types for different applications

  | Live Chat | Ticketing And Messaging | Knowledge Base | Bot|	Global |
  | - | - | - | - | - |
  | audioAndVideoChatManagement | autoAllocationManagement | categoryManagement | agentAssistLearningManagement | accountProfileManagement |
  | autoAcceptSetting | blockSenderManagement | articleManagement | agentAssistSettingManagement | agentManagement |
  | autoInvitationManagement | channelIntegrationManagement | tagManagement | agentAssistSynonymManagement | agentRoleManagement |
  | autoTranslationManagement | conversationManagement | imageManagement | botSettings | applicationsManagement |
  | banManagement | fieldsAndMappingsManagement | designManagement | botsManagement | billingInfoManagement |
  | campaignsManagement |ticketAndMessagingRoutingRulesManagement | customPageManagement | entitiesManagement | cannedMessageManagement |
  | chatsAuto-AllocationManagement | slaPoliciesManagement | knowledgeBaseManagement | intentsManagement | customAwayStatusManagement |
  | chatSettingsManagement | triggerManagement | | quickRepliesManagement | customerAccountsManagement |
  | cobrowsingManagement | workingTimeAndHolidaysManagement | | smartTriggersManagement | departmentManagement |
  | conversionManagement | | | visitorQuestioninLearningDelete | integrationAndAPIManagement |
  | customVariableManage | | | | licenseManagement |
  | dashboardCustomMetricManagement | | | | manageAccountStatusorState |
  | liveChatRoutingRulesManagement | | | | manuallyChargeandActiveAccount |
  | secureFormManagement | | | | restrictedWordsManagement |
  | segmentationManagement | | | | securityManagement |
  | shiftsManagement | | | | siteProfileManagement |
  | transcriptDelete | | | |   |
  | visitorSSOManagement | | | |   |

## Audit Log Endpoints

### Get audit logs list

  `GET /api/v3/globalSettings/auditLogs`

  #### Parameters

   Query stringno

  | Name  | Type | Required  | Default | Description |
  | - | - | - | - | - |
  |`dateFrom`|DateTime|no||The date from which agent did the action, format as yyyy-MM-ddTHH:mm:ss. |
  |`dateTo`|DateTime|no||The date when an agent ended the action, format as yyyy-MM-ddTHH:mm:ss. |
  |`category`|string|no||The category which the action belongs to |
  |`actionType`|string|no||The action type. |
  |`agentId`|integer |no||id of the agent who did the action. |
  |`keywords`|string|no||The key words associated with the action. |
  |`pageIndex`|integer|no| 1 | The page index of the query. |
  |`pageSize`|integer|no| 10 |The page size of the query. |
  |`include`|string|no||Available value: `agent` |


#### Response

  The response is a list of [Audit Log List Response](#audit-log-list-response-object) Objects

  #### Example
Using curl
```
curl -H "Content-Type: application/json" -X GET https://api1.comm100.io/api/v3/globalSettings/auditLogs
```
Response
```json
HTTP/1.1 200 OK
Content-Type:  application/json
{
    "count": 1234,
    "nextPage": "https://api1.comm100.io/api/v3/globalSettings/auditLogs?pageIndex=2",
    "previousPage": "",
    "auditLogs": [
      {
        "id": 201,
        "name": "add Agent",
        "category": "My Account Global Settings",
        "actionTime": "2020-02-02",
        "createdBy": 68,
        "actionType": "Add Agent",
        "actionSummary": "Add Agent",
        "actionDetails": "Add Agent for Live Chat",
      },
      ...,
      ]
}
```




# Time Zone Options


  |   Id    |   Display name   |
  |   -   |   -   |
  |	Morocco Standard Time	|	(GMT) Casablanca	|
  |	UTC	|	(GMT) Coordinated Universal Time	|
  |	GMT Standard Time	|	(GMT) Greenwich Mean Time : Dublin, Edinburgh, Lisbon, London	|
  |	Greenwich Standard Time	|	(GMT) Monrovia, Reykjavik	|
  |	W. Europe Standard Time	|	(GMT+01:00) Amsterdam, Berlin, Bern, Rome, Stockholm, Vienna	|
  |	Central Europe Standard Time	|	(GMT+01:00) Belgrade, Bratislava, Budapest, Ljubljana, Prague	|
  |	Romance Standard Time	|	(GMT+01:00) Brussels, Copenhagen, Madrid, Paris	|
  |	Central European Standard Time	|	(GMT+01:00) Sarajevo, Skopje, Warsaw, Zagreb	|
  |	W. Central Africa Standard Time	|	(GMT+01:00) West Central Africa	|
  |	Namibia Standard Time	|	(GMT+01:00) Windhoek	|
  |	Jordan Standard Time	|	(GMT+02:00) Amman	|
  |	GTB Standard Time	|	(GMT+02:00) Athens, Bucharest, Istanbul	|
  |	Middle East Standard Time	|	(GMT+02:00) Beirut	|
  |	Egypt Standard Time	|	(GMT+02:00) Cairo	|
  |	South Africa Standard Time	|	(GMT+02:00) Harare, Pretoria	|
  |	FLE Standard Time	|	(GMT+02:00) Helsinki, Kyiv, Riga, Sofia, Tallinn, Vilnius	|
  |	Israel Standard Time	|	(GMT+02:00) Jerusalem	|
  |	E. Europe Standard Time	|	(GMT+02:00) Minsk	|
  |	Arabic Standard Time	|	(GMT+03:00) Baghdad	|
  |	Arab Standard Time	|	(GMT+03:00) Kuwait, Riyadh	|
  |	Russian Standard Time	|	(GMT+03:00) Moscow, St. Petersburg, Volgograd	|
  |	E. Africa Standard Time	|	(GMT+03:00) Nairobi	|
  |	Iran Standard Time	|	(GMT+03:30) Tehran	|
  |	Arabian Standard Time	|	(GMT+04:00) Abu Dhabi, Muscat	|
  |	Azerbaijan Standard Time	|	(GMT+04:00) Baku	|
  |	Mauritius Standard Time	|	(GMT+04:00) Port Louis	|
  |	Georgian Standard Time	|	(GMT+04:00) Tbilisi	|
  |	Caucasus Standard Time	|	(GMT+04:00) Yerevan	|
  |	Afghanistan Standard Time	|	(GMT+04:30) Kabul	|
  |	Ekaterinburg Standard Time	|	(GMT+05:00) Ekaterinburg	|
  |	Pakistan Standard Time	|	(GMT+05:00) Islamabad, Karachi	|
  |	West Asia Standard Time	|	(GMT+05:00) Tashkent	|
  |	India Standard Time	|	(GMT+05:30) Chennai, Kolkata, Mumbai, New Delhi	|
  |	Sri Lanka Standard Time	|	(GMT+05:30) Sri Jayawardenepura	|
  |	Nepal Standard Time	|	(GMT+05:45) Kathmandu	|
  |	Bangladesh Standard Time	|	(GMT+06:00) Astana, Dhaka	|
  |	N. Central Asia Standard Time	|	(GMT+06:00) Novosibirsk	|
  |	Myanmar Standard Time	|	(GMT+06:30) Yangon (Rangoon)	|
  |	SE Asia Standard Time	|	(GMT+07:00) Bangkok, Hanoi, Jakarta	|
  |	North Asia Standard Time	|	(GMT+07:00) Krasnoyarsk	|
  |	China Standard Time	|	(GMT+08:00) Beijing, Chongqing, Hong Kong, Urumqi	|
  |	North Asia East Standard Time	|	(GMT+08:00) Irkutsk	|
  |	Singapore Standard Time	|	(GMT+08:00) Kuala Lumpur, Singapore	|
  |	W. Australia Standard Time	|	(GMT+08:00) Perth	|
  |	Taipei Standard Time	|	(GMT+08:00) Taipei	|
  |	Ulaanbaatar Standard Time	|	(GMT+08:00) Ulaanbaatar	|
  |	Tokyo Standard Time	|	(GMT+09:00) Osaka, Sapporo, Tokyo	|
  |	Korea Standard Time	|	(GMT+09:00) Seoul	|
  |	Yakutsk Standard Time	|	(GMT+09:00) Yakutsk	|
  |	Cen. Australia Standard Time	|	(GMT+09:30) Adelaide	|
  |	AUS Central Standard Time	|	(GMT+09:30) Darwin	|
  |	E. Australia Standard Time	|	(GMT+10:00) Brisbane	|
  |	AUS Eastern Standard Time	|	(GMT+10:00) Canberra, Melbourne, Sydney	|
  |	West Pacific Standard Time	|	(GMT+10:00) Guam, Port Moresby	|
  |	Tasmania Standard Time	|	(GMT+10:00) Hobart	|
  |	Vladivostok Standard Time	|	(GMT+10:00) Vladivostok	|
  |	Central Pacific Standard Time	|	(GMT+11:00) Magadan, Solomon Is., New Caledonia	|
  |	New Zealand Standard Time	|	(GMT+12:00) Auckland, Wellington	|
  |	Fiji Standard Time	|	(GMT+12:00) Fiji, Marshall Is.	|
  |	Kamchatka Standard Time	|	(GMT+12:00) Petropavlovsk-Kamchatsky	|
  |	Samoa Standard Time	|	(GMT+13:00) Midway Island, Samoa	|
  |	Tonga Standard Time	|	(GMT+13:00) Nuku'alofa	|
  |	Azores Standard Time	|	(GMT-01:00) Azores	|
  |	Cape Verde Standard Time	|	(GMT-01:00) Cape Verde Is.	|
  |	Mid-Atlantic Standard Time	|	(GMT-02:00) Mid-Atlantic	|
  |	E. South America Standard Time	|	(GMT-03:00) Brasilia	|
  |	Argentina Standard Time	|	(GMT-03:00) Buenos Aires	|
  |	SA Eastern Standard Time	|	(GMT-03:00) Cayenne	|
  |	Greenland Standard Time	|	(GMT-03:00) Greenland	|
  |	Montevideo Standard Time	|	(GMT-03:00) Montevideo	|
  |	Newfoundland Standard Time	|	(GMT-03:30) Newfoundland	|
  |	Paraguay Standard Time	|	(GMT-04:00) Asuncion	|
  |	Atlantic Standard Time	|	(GMT-04:00) Atlantic Time (Canada)	|
  |	SA Western Standard Time	|	(GMT-04:00) Georgetown, La Paz, San Juan	|
  |	SA Western Standard Time	|	(GMT-04:00) Manaus	|
  |	Pacific SA Standard Time	|	(GMT-04:00) Santiago	|
  |	Venezuela Standard Time	|	(GMT-04:30) Caracas	|
  |	SA Pacific Standard Time	|	(GMT-05:00) Bogota, Lima, Quito	|
  |	Eastern Standard Time	|	(GMT-05:00) Eastern Time (US & Canada)	|
  |	US Eastern Standard Time	|	(GMT-05:00) Indiana (East)	|
  |	Central America Standard Time	|	(GMT-06:00) Central America	|
  |	Central Standard Time	|	(GMT-06:00) Central Time (US & Canada)	|
  |	Central Standard Time (Mexico)	|	(GMT-06:00) Guadalajara, Mexico City, Monterrey	|
  |	Canada Central Standard Time	|	(GMT-06:00) Saskatchewan	|
  |	US Mountain Standard Time	|	(GMT-07:00) Arizona	|
  |	Mountain Standard Time (Mexico)	|	(GMT-07:00) Chihuahua, La Paz, Mazatlan	|
  |	Mountain Standard Time	|	(GMT-07:00) Mountain Time (US & Canada)	|
  |	Pacific Standard Time	|	(GMT-08:00) Pacific Time (US & Canada)	|
  |	Pacific Standard Time (Mexico)	|	(GMT-08:00) Tijuana, Baja California	|
  |	Alaskan Standard Time	|	(GMT-09:00) Alaska	|
  |	Hawaiian Standard Time	|	(GMT-10:00) Hawaii	|
  |	Dateline Standard Time	|	(GMT-12:00) International Date Line West	|



