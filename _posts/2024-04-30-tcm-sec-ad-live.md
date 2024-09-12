---
layout: post
title:  "Write Up: Proving Grounds Practice - Exfiltrated"
date:   2024-07-23 17:37:51 -0400
categories: TCMSecurity AD class-notes
---
 Notes from taking the TCM Security AD Live course on 26-04-2024.

# Introduction

## What We'll Cover

AD Overview & Pre-Compromise Attacks
- What is AD
- LLMNR Poisening Attacks
- SMB & IPv6 Relay Attacks
- AS-REP Roasting Attacks
- Passback Attacks

Post-Compromise AD Enumeration
- Reviewing common enum tools such as: bloodhound, plumhound, pingcsatle, ldapdomaindump, etc
- Understanding common mistakes from an admin perspective

Post-Compromise AD Attacks
- pass attacks
- kerberoasting 
- token impersonatioon
- url file attacks
- GPP Attacks

Post-Compromise AD Attacks Contin
- credential dumping attacks
- persistence attacks
- report writing
- ad case studies

## Class Objectives
- learn to hack, practically
- learn to defend
- gain insight into the mindset
- prepare for a job in the field

## Engagement Steps
- SOW (statement of work) & MSA (master service agreement) Signed
- ROE (rules of engagement) signed
- Scope verified
- pentest occurs
- report written and delivered
- client debrief
- retesting (if needed)

## Five STAGES of hacking
1. recon (active & passive)
2. Scanning & Enumeration
3. Gaining Access
4. Maintaining Access
5. Covering trqcks

## AD Overview

### What is active directory?
- directory service created by microsoft to manage windows domain networks
- stores info related to objects, such as computers, users, printers, etc. Think about it as phone book for windows
- authenitcates using kerberos. Non-windows devices such as linux machines, direwalls, etc can also authenticate to active directory via RADIOS or LDAP
- Active Directory is the most commonly used id management service in the world; ~95% of fortune 1000 companies implement the service in their networks
- Can be xploited without ever attacking patchable exploits; instead we abuse features, trust, components, and more

### Components

|Physical|Logical|
|--------|-------|
|Data store|Partitions|
|Domain Controllers|Schema|
|Global catalog server|Domains|
|Read only domain controller (RODC)|Domain Tree|
| |Forests|
| |Sites|
| | Organziational Units (OUs)|

### Domain Controllers
A Domain Controller is a server with the AD DS server role installed that has been specifically promoted to a domain controller. Domain controllers:
- host a copy of the AD DS directory store
- provide authentication and authorization services
- replicated updates to other domain controllers in the domain and forest
- allow administrative access to manage user accounts and network resources

### AD DS Schema
- defines every type of object that can be stored in the directory
- enforces rules regarding object creation and configuration

|Object Types|Function|Examples|
|------------|--------|--------|
|Class Object | what objects can be created in the directory | - user\n- computer|
|Attribute Object | Information that can be attached to an object | - Display name|


### Domains
Domains are used to group and manage objects in an org. Domains:
- Ad admin boundary for applying policies to groups of objects
- A replication boundary for replicating data between domain controllers
- An authentication and authorization boundary that provides a way to limit the scope of access to resources

### Trees
A domain tree is a hierarchy of domains in AD DS. All domains in the tree:
- share a contiguous namespace with the parent domain
- can have additional child domains
- by default create a two-way transitive trust with other domains

### Forests
A forest is a collection od one or more domain trees. Forests:
- share a common schema
- share a common config partition
- share a common global catalog to enable searching
- enable trust between all domains in the firest
- share enterprise admin and schemaa admin groups

### Organizational Units
OUs are Active Directory containers that can contain users, groups, computers, and other OUs. OUs are used to:
- represent your org hierachically and logically
- manage a collection of objects in a consistent way
- delegate permissions to administer groups of objects
- apply policies

### Trusts
Trusts provide a mechanism for users to gain access to resources in another domain
- directional : the trust direction flows from trusting domain to trusted domain
- transitive : the trust relationship is extended beyond a two-domain trust to include other trusted domains
- all domains in a forest trust all other domains ina forest
- trusts can be extended outside the forest

# Initial AD Attack Vectors

## LLMNR Poisoning

### WHat is LLMNR?
- Used to id hosts when dns fails to do so
- Previously NBT-NS
- Key flaw is that the services utilze a user's username and NTLMv2 hash when appropriately responded to.

### Example LLMNR Poisoning
1. Run Responder

> sudo responder -I tun0 -dPv

2. Some one on the network triggers an event

3. On the event responder will print out the NTLM hash

4. Crack the hash

> hashcat -m 5600 hashes.txt rockyou.txt