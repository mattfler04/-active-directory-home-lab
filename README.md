# Active Directory Home Lab

## Overview

I built a Windows Server 2025 Active Directory home lab using UTM on a Mac mini.

The purpose of this project is to gain hands-on experience with Windows Server administration, Active Directory, DNS, Group Policy, user and group management, domain joining, network troubleshooting, Help Desk troubleshooting, account security, and Windows Event Viewer.

This project focuses on building a working Windows domain environment, troubleshooting real configuration problems, documenting the solutions, and practicing common IT Support scenarios.

---

# Lab Environment

## Host Computer

- Mac mini
- 24 GB RAM
- UTM virtualization
- External SSD used for virtual machine storage

## Server

- Windows Server 2025
- Computer Name: `DC01`
- Domain Controller
- Active Directory Domain Services
- DNS
- Global Catalog

## Client

- Windows 11 Pro
- Domain client
- Connected to the same UTM virtual network as DC01

## Domain

`matthias.local`

## Domain Controller

`DC01`

IP Address:

`192.168.64.2`

---

# Lab Architecture

```text
                 Active Directory Domain
                      matthias.local
                            |
                            |
                         DC01
                  Windows Server 2025
                    192.168.64.2
                            |
             -----------------------------
             |                           |
            DNS                    Active Directory
             |                           |
             -----------------------------
                            |
                            |
                    Windows 11 Client
                            |
                       Domain User
                        ajohnson