jCasbin
====

[![codebeat badge](https://codebeat.co/badges/c17c9ee1-da42-4db3-8047-9574ad2b23b1)](https://codebeat.co/projects/github-com-casbin-jcasbin-master)
[![Build Status](https://travis-ci.org/casbin/jcasbin.svg?branch=master)](https://travis-ci.org/casbin/jcasbin)
[![Coverage Status](https://coveralls.io/repos/github/casbin/jcasbin/badge.svg?branch=master)](https://coveralls.io/github/casbin/jcasbin?branch=master)
[![Javadocs](https://www.javadoc.io/badge/org.casbin/jcasbin.svg)](https://www.javadoc.io/doc/org.casbin/jcasbin)
[![Release](https://img.shields.io/github/release/casbin/jcasbin.svg)](https://github.com/casbin/jcasbin/releases/latest)
[![Gitter](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/casbin/lobby)
[![Patreon](https://img.shields.io/badge/patreon-donate-yellow.svg)](http://www.patreon.com/yangluo)
[![Sourcegraph Badge](https://sourcegraph.com/github.com/casbin/casbin/-/badge.svg)](https://sourcegraph.com/github.com/casbin/casbin?badge)

**News**: still worry about how to write the correct jCasbin policy? ``Casbin online editor`` is coming to help! Try it at: http://casbin.org/editor/

![casbin Logo](casbin-logo.png)

jCasbin is a powerful and efficient open-source access control library for Java projects. It provides support for enforcing authorization based on various [access control models](https://en.wikipedia.org/wiki/Computer_security_model). This project is a Java port of the original Golang [Casbin](https://github.com/casbin/casbin).

### Supported by Auth0 <span><img src="https://user-images.githubusercontent.com/1801923/31792116-d4fca9ec-b512-11e7-92eb-56e8d3df8e70.png" height="28" align="top"></span>

If you want to easily add authentication and authorization to your Go projects, feel free to check out Auth0's Go SDK and free plan at [auth0.com/overview](https://auth0.com/overview?utm_source=GHsponsor&utm_medium=GHsponsor&utm_campaign=casbin&utm_content=auth)

## Table of contents

- [Supported models](#supported-models)
- [How it works?](#how-it-works)
- [Features](#features)
- [Installation](#installation)
- [Documentation](#documentation)
- [Online editor](#online-editor)
- [Tutorials](#tutorials)
- [Get started](#get-started)
- [Policy management](#policy-management)
- [Policy persistence](#policy-persistence)
- [Role manager](#role-manager)
- [Examples](#examples)
- [Our adopters](#our-adopters)

## Supported models

1. [**ACL (Access Control List)**](https://en.wikipedia.org/wiki/Access_control_list)
2. **ACL with [superuser](https://en.wikipedia.org/wiki/Superuser)**
3. **ACL without users**: especially useful for systems that don't have authentication or user log-ins.
3. **ACL without resources**: some scenarios may target for a type of resources instead of an individual resource by using permissions like ``write-article``, ``read-log``. It doesn't control the access to a specific article or log.
4. **[RBAC (Role-Based Access Control)](https://en.wikipedia.org/wiki/Role-based_access_control)**
5. **RBAC with resource roles**: both users and resources can have roles (or groups) at the same time.
6. **RBAC with domains/tenants**: users can have different role sets for different domains/tenants.
7. **[ABAC (Attribute-Based Access Control)](https://en.wikipedia.org/wiki/Attribute-Based_Access_Control)**: syntax sugar like ``resource.Owner`` can be used to get the attribute for a resource.
8. **[RESTful](https://en.wikipedia.org/wiki/Representational_state_transfer)**: supports paths like ``/res/*``, ``/res/:id`` and HTTP methods like ``GET``, ``POST``, ``PUT``, ``DELETE``.
9. **Deny-override**: both allow and deny authorizations are supported, deny overrides the allow.
10. **Priority**: the policy rules can be prioritized like firewall rules.

## How it works?

In jCasbin, an access control model is abstracted into a CONF file based on the **PERM metamodel (Policy, Effect, Request, Matchers)**. So switching or upgrading the authorization mechanism for a project is just as simple as modifying a configuration. You can customize your own access control model by combining the available models. For example, you can get RBAC roles and ABAC attributes together inside one model and share one set of policy rules.

The most basic and simplest model in jCasbin is ACL. ACL's model CONF is:

```ini
# Request definition
[request_definition]
r = sub, obj, act

# Policy definition
[policy_definition]
p = sub, obj, act

# Policy effect
[policy_effect]
e = some(where (p.eft == allow))

# Matchers
[matchers]
m = r.sub == p.sub && r.obj == p.obj && r.act == p.act
```

An example policy for ACL model is like:

```
p, alice, data1, read
p, bob, data2, write
```

It means:

- alice can read data1
- bob can write data2

## Features

What jCasbin does:

1. enforce the policy in the classic ``{subject, object, action}`` form or a customized form as you defined, both allow and deny authorizations are supported.
2. handle the storage of the access control model and its policy.
3. manage the role-user mappings and role-role mappings (aka role hierarchy in RBAC).
4. support built-in superuser like ``root`` or ``administrator``. A superuser can do anything without explict permissions.
5. multiple built-in operators to support the rule matching. For example, ``keyMatch`` can map a resource key ``/foo/bar`` to the pattern ``/foo*``.

What jCasbin does NOT do:

1. authentication (aka verify ``username`` and ``password`` when a user logs in)
2. manage the list of users or roles. I believe it's more convenient for the project itself to manage these entities. Users usually have their passwords, and jCasbin is not designed as a password container. However, jCasbin stores the user-role mapping for the RBAC scenario. 

## Installation

For Maven:

```
<dependency>
  <groupId>org.casbin</groupId>
  <artifactId>jcasbin</artifactId>
  <version>0.0.4-FIX</version>
</dependency>
```

## Documentation

For documentation, please see: [Our Wiki](https://github.com/casbin/casbin/wiki)

## Online editor

You can also use the online editor (http://casbin.org/editor/) to write your jCasbin model and policy in your web browser. It provides functionality such as ``syntax highlighting`` and ``code completion``, just like an IDE for a programming language.

## Tutorials

- [Basic Role-Based HTTP Authorization in Go with Casbin](https://zupzup.org/casbin-http-role-auth) (or [Chinese translation](https://studygolang.com/articles/12323))
- [Using Casbin with Beego: 1. Get started and test (in Chinese)](http://blog.csdn.net/hotqin888/article/details/78460385)
- [Using Casbin with Beego: 2. Policy storage (in Chinese)](http://blog.csdn.net/hotqin888/article/details/78571240)
- [Using Casbin with Beego: 3. Policy query (in Chinese)](http://blog.csdn.net/hotqin888/article/details/78992250)

## Get started

1. New a jCasbin enforcer with a model file and a policy file:

    ```java
    Enforcer enforcer = new Enforcer("path/to/model.conf", "path/to/policy.csv");
    ```

Note: you can also initialize an enforcer with policy in DB instead of file, see [Persistence](#persistence) section for details.

2. Add an enforcement hook into your code right before the access happens:

    ```java
    String sub = "alice"; // the user that wants to access a resource.
    String obj = "data1"; // the resource that is going to be accessed.
    String act = "read"; // the operation that the user performs on the resource.

    if (enforcer.enforce(sub, obj, act) == true) {
        // permit alice to read data1
    } else {
        // deny the request, show an error
    }
    ```

3. Besides the static policy file, jCasbin also provides API for permission management at run-time. For example, You can get all the roles assigned to a user as below:

    ```java
    Roles roles = enforcer.getRoles("alice");
    ```

See [Policy management APIs](#policy-management) for more usage.

4. Please refer to the [src/test](https://github.com/casbin/jcasbin/tree/master/src/test) package for more usage.

## Policy management

jCasbin provides two sets of APIs to manage permissions:

- [Management API](https://github.com/casbin/jcasbin/blob/master/src/main/java/org/casbin/jcasbin/main/ManagementEnforcer.java): the primitive API that provides full support for jCasbin policy management. See [here](https://github.com/casbin/jcasbin/blob/master/src/test/java/org/casbin/jcasbin/main/ManagementAPIUnitTest.java) for examples.
- [RBAC API](https://github.com/casbin/jcasbin/blob/master/src/main/java/org/casbin/jcasbin/main/Enforcer.java): a more friendly API for RBAC. This API is a subset of Management API. The RBAC users could use this API to simplify the code. See [here](https://github.com/casbin/jcasbin/blob/master/src/test/java/org/casbin/jcasbin/main/RbacAPIUnitTest.java) for examples.

We also provide a web-based UI for model management and policy management:

![model editor](https://hsluoyz.github.io/casbin/ui_model_editor.png)

![policy editor](https://hsluoyz.github.io/casbin/ui_policy_editor.png)

## Policy persistence

In jCasbin, the policy storage is implemented as an adapter (aka middleware for jCasbin). To keep light-weight, we don't put adapter code in the main library (except the default file adapter). A complete list of jCasbin adapters is provided as below. Any 3rd-party contribution on a new adapter is welcomed, please inform us and I will put it in this list:)

Adapter | Type | Author | Description
----|------|----|----
[File Adapter (built-in)](https://github.com/casbin/casbin/wiki/Policy-persistence#file-adapter) | File | jCasbin | Persistence for [.CSV (Comma-Separated Values)](https://en.wikipedia.org/wiki/Comma-separated_values) files

For details of adapters, please refer to the documentation: https://github.com/casbin/casbin/wiki/Policy-persistence

## Role manager

The role manager is used to manage the RBAC role hierarchy (user-role mapping) in jCasbin. A role manager can retrieve the role data from jCasbin policy rules or external sources such as LDAP, Okta, Auth0, Azure AD, etc. We support different implementations of a role manager. To keep light-weight, we don't put role manager code in the main library (except the default role manager). A complete list of jCasbin role managers is provided as below. Any 3rd-party contribution on a new role manager is welcomed, please inform us and I will put it in this list:)

Role manager | Author | Description
----|----|----
[Default Role Manager (built-in)](https://github.com/casbin/jcasbin/blob/master/src/main/java/org/casbin/jcasbin/rbac/DefaultRoleManager.java) | jCasbin | Supports role hierarchy stored in jCasbin policy

For developers: all role managers must implement the [RoleManager](https://github.com/casbin/jcasbin/blob/master/src/main/java/org/casbin/jcasbin/rbac/RoleManager.java) interface. [Default Role Manager](https://github.com/casbin/jcasbin/blob/master/src/main/java/org/casbin/jcasbin/rbac/DefaultRoleManager.java) can be used as a reference implementation.

## Examples

Model | Model file | Policy file
----|------|----
ACL | [basic_model.conf](https://github.com/casbin/jcasbin/blob/master/examples/basic_model.conf) | [basic_policy.csv](https://github.com/casbin/jcasbin/blob/master/examples/basic_policy.csv)
ACL with superuser | [basic_model_with_root.conf](https://github.com/casbin/jcasbin/blob/master/examples/basic_with_root_model.conf) | [basic_policy.csv](https://github.com/casbin/jcasbin/blob/master/examples/basic_policy.csv)
ACL without users | [basic_model_without_users.conf](https://github.com/casbin/jcasbin/blob/master/examples/basic_without_users_model.conf) | [basic_policy_without_users.csv](https://github.com/casbin/jcasbin/blob/master/examples/basic_without_users_policy.csv)
ACL without resources | [basic_model_without_resources.conf](https://github.com/casbin/jcasbin/blob/master/examples/basic_without_resources_model.conf) | [basic_policy_without_resources.csv](https://github.com/casbin/jcasbin/blob/master/examples/basic_without_resources_policy.csv)
RBAC | [rbac_model.conf](https://github.com/casbin/jcasbin/blob/master/examples/rbac_model.conf)  | [rbac_policy.csv](https://github.com/casbin/jcasbin/blob/master/examples/rbac_policy.csv)
RBAC with resource roles | [rbac_model_with_resource_roles.conf](https://github.com/casbin/jcasbin/blob/master/examples/rbac_with_resource_roles_model.conf)  | [rbac_policy_with_resource_roles.csv](https://github.com/casbin/jcasbin/blob/master/examples/rbac_with_resource_roles_policy.csv)
RBAC with domains/tenants | [rbac_model_with_domains.conf](https://github.com/casbin/jcasbin/blob/master/examples/rbac_with_domains_model.conf)  | [rbac_policy_with_domains.csv](https://github.com/casbin/jcasbin/blob/master/examples/rbac_with_domains_policy.csv)
ABAC | [abac_model.conf](https://github.com/casbin/jcasbin/blob/master/examples/abac_model.conf)  | N/A
RESTful | [keymatch_model.conf](https://github.com/casbin/jcasbin/blob/master/examples/keymatch_model.conf)  | [keymatch_policy.csv](https://github.com/casbin/jcasbin/blob/master/examples/keymatch_policy.csv)
Deny-override | [rbac_model_with_deny.conf](https://github.com/casbin/jcasbin/blob/master/examples/rbac_with_deny_model.conf)  | [rbac_policy_with_deny.csv](https://github.com/casbin/jcasbin/blob/master/examples/rbac_with_deny_policy.csv)
Priority | [priority_model.conf](https://github.com/casbin/jcasbin/blob/master/examples/priority_model.conf)  | [priority_policy.csv](https://github.com/casbin/jcasbin/blob/master/examples/priority_policy.csv)

## Our adopters

### Web frameworks

- [Spring Boot](https://projects.spring.io/spring-boot/): Makes it easy to create Spring-powered applications and services, via plugin: [jcasbin-springboot-plugin](https://github.com/jcasbin/jcasbin-springboot-plugin)
- [Vert.x](https://vertx.io/): A tool-kit for building reactive applications on the JVM, via plugin: [vertx-auth-jcasbin](https://github.com/vert-x3/vertx-auth/pull/215)
- [JFinal](http://www.jfinal.com/): A simple, light, rapid, independent and extensible Java WEB + ORM framework, via plugin: [jcasbin-jfinal-plugin](https://github.com/jcasbin/jcasbin-jfinal-plugin)
- [Nutz](https://nutzam.com/): Web framework (MVC/IOC/AOP/DAO/JSON) for all Java developers, via plugin: [jcasbin-nutz-plugin](https://github.com/jcasbin/jcasbin-nutz-plugin)

## License

This project is licensed under the [Apache 2.0 license](https://github.com/casbin/jasbin/blob/master/LICENSE).

## Contact

If you have any issues or feature requests, please contact us. PR is welcomed.
- https://github.com/casbin/jcasbin/issues
- hsluoyz@gmail.com
- Tencent QQ group: [546057381](//shang.qq.com/wpa/qunwpa?idkey=8ac8b91fc97ace3d383d0035f7aa06f7d670fd8e8d4837347354a31c18fac885)
