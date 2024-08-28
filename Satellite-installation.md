**Red Hat Satellite 6.11.5** 

**Installation**

**Documentation**

    ****

    ****


## This Document includes all the steps performed for the setup.

\



# 1. Download the RHEL 8.10 Image:

Obtain the rhel-8.10-x86\_64-kvm.qcow2 image from the official Red Hat website.

\
\
\
\


Setting Up an HTTP Repository Server to Serve RHEL Packages


# Objective

To create an HTTP repository server, allowing client machines to download and install Red Hat Enterprise Linux packages using reposync.


# Prerequisites

1. A Red Hat Enterprise Linux (RHEL) system.

2. Proper RHEL subscription attached to the machine.


## Procedure

# 1.Setting Up the Hostname and Registering with Subscription Manager:

|                                                                                                                                              |
| -------------------------------------------------------------------------------------------------------------------------------------------- |
| **#** subscription-manager register## # hostnamectl set-hostname satellite.example.com## # subscription-manager refresh## # yum check-update |


# 2. Enabling Repositories for Download:

\# Enable desired repositories which you want to sync to your local server.

|                                              |
| -------------------------------------------- |
| ## # subscription-manager repos --list rgdgd |

\# Enable any other repositories as needed

|                                                                                                                                                                                                                                             |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ## subscription-manager repos --enable=rhel-8-for-x86\_64-baseos-rpms \\## --enable=rhel-8-for-x86\_64-appstream-rpms \\## --enable=satellite-6.11-for-rhel-8-x86\_64-rpms \\## --enable=satellite-maintenance-6.11-for-rhel-8-x86\_64-rpms |


# 3. Install Required Packages:

Install yum-utils which provides reposync, and httpd for setting up the web server.

\


|                                      |
| ------------------------------------ |
| ## # **yum install yum-utils httpd** |

\
\



# 6. Setting Up Firewall Rules:

## Allow httpd through the firewall:

\


|                                                                                                                                  |
| -------------------------------------------------------------------------------------------------------------------------------- |
| ## #**firewall-cmd --add-service=http --permanent** **#firewall-cmd --add-service=https --permanent** **#firewall-cmd --reload** |

\
\
\


Red Hat Satellite 6 Installation Preparation 


## Document Purpose:

This guide provides step-by-step instructions to prepare a server environment for the installation of Red Hat Satellite 6.


# 1. Preparing Your Environment for Installation

## 1.1. Host System Requirements

The server hosting Red Hat Satellite 6 must adhere to the following minimum specifications:

- CPU architecture: x86\_64

- CPU cores: Minimum of four, each at 2.0 GHz

- Physical memory: Minimum of 20 GB

- Swap space: Minimum of 4 GB


## 1.2. Security Configuration

For software deployment, configuration management, and provisioning, allow the following network traffic:

To configure the necessary network ports, run the following commands:

\


|                                                                                                                                                                                                                                                                                                                                                                                                                |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| \[root\@setellite naveen]# **firewall-cmd --permanent \\** **--add-port="7/udp" --add-port="7/tcp" \\** **--add-port="53/udp" --add-port="53/tcp" \\****** **--add-port="67/udp" --add-port="68/udp" \\**  **--add-port="69/udp" --add-port="80/tcp" \\** **--add-port="443/tcp" --add-port="5646/tcp" \\** **--add-port="5671/tcp" --add-port="8000/tcp" \\** **--add-port="8140/tcp" --add-port="9090/tcp"** |

\
\


|                                                       |
| ----------------------------------------------------- |
| \[root\@setellite naveen]# **firewall-cmd --reload**  |

Ensure SELinux permissions are set to enforcing:

\


|                                           |
| ----------------------------------------- |
| \[root\@setellite naveen]# **getenforce** |

The output should display: Enforcing.

For accurate time synchronization, install and enable chronyd on the RHEL host:

\


|                                                         |
| ------------------------------------------------------- |
| \[root\@setellite naveen]# **systemctl status chronyd** |

\



# 2. Verifying DNS Resolution

## 2.1. Verification Procedure

Ensure that the host name and localhost resolve correctly:

|                                                        |
| ------------------------------------------------------ |
| # **ping -c1 localhost**# **ping -c1 \`hostname -f\`** |

The output should reflect successful name resolution.


# 2.2. Setting Hostnames

To ensure consistency in naming:

\


|                                                      |
| ---------------------------------------------------- |
| # **hostnamectl set-hostname satellite.example.com** |

\
\



# 4. Synchronizing the System Clock with Chronyd

1. ## Installing the Chrony Package:

Kick-start the process by installing the required package for clock synchronization:

|                          |
| ------------------------ |
| **# yum install chrony** |

2. ## Activating the Chronyd Service:

Enable and initiate the chronyd service to ensure time synchronization:

|                                      |
| ------------------------------------ |
| **# systemctl enable --now chronyd** |


# 5. Installing the SOS Package on the Base Operating System

Installation of the SOS Package:

Install the sos package to equip the system with diagnostic capabilities:

|                       |
| --------------------- |
| **# yum install sos** |

\



# 6.Configuring Satellite Installation

Objective:

The initial configuration of the Satellite installation involves setting up an organization, location, user name, and password. This configuration lays down the foundational settings required for the smooth functioning of the Satellite Server.

Procedure:

1. ## Setting Up Initial Configuration:

Initiate the configuration with the following command, adjusting values as per your requirements:

\


|                                                                                                                                                                                                                                         |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **# satellite-installer --scenario satellite \\**  **--foreman-initial-organization "keenable" \\**  **--foreman-initial-location "Noida" \\**  **--foreman-initial-admin-username Naveen\\**  **--foreman-initial-admin-password xyz** |

****![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfqZLmSYWzsosuv58qDBfSgsNQg9qyPIws4lzaj5i9jPNKR9GO4L-3CnC_APTlG3DDcu-zGTsc82KhCBDl81nNPSdoegeF_WfZtFB3hRxgT-hb06EjvHb1JCG_mRBu34fKo-0qCgw-ZUL5PY01azUp7IYYU?key=LSSB9efPNxKCtrn5UlVYTA)****

2. ## Validate a Satellite Server Installation

You can view and query a Satellite Server to verify that it is successfully installed and operational.

Use the Satellite Server web UI and login with the credentials or the Hammer CLI to query the central Satellite Server.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXew2pu00NGQUk9CjlUIIVSXy7HanKXGE9VGIbYRkKqDfVf2OxzvuJJDUMuSsHmk0vyvO24HIy_ZBQ3K1167DDVXrIxIA8WWZIYi2KbPlQYdp63XfQrKEN-XOm5gW7UBljJSJl2hbyy87Wuwl27vWozLuNjy?key=LSSB9efPNxKCtrn5UlVYTA)

\
\
\


Satellite Additional Configuration


# Configuring Organizations and Locations in Red Hat Satellite 6

Manage Organizations:


## Create:

- Go to Administer > Organizations > New Organization.

- Enter name, unique label (no spaces), and optional description.


## Edit:

- Navigate to Administer > Organizations.

- Click the organization name or Edit in the Actions column.

- Adjust details in the categories.


## Delete:

- Navigate to Administer > Organizations.

- Select Delete next to desired organization.


## Manage Locations:

## Create:

- Go to Administer > Locations > New Location.

- Choose parent if it's a sub-location and enter a name.


## Edit:

- Navigate to Administer > Locations.

- Click location name or Edit in Actions column.

- Modify details as needed.


## Delete:

- Ensure location isn't linked to lifecycle environments or host groups.

- Go to Administer > Locations and select Delete.


# Managing Subscriptions and Content in Red Hat Satellite Server

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcfAQWB24f3KiimDcXVxRILS8qarz9N7yU0l3sUIPZb4fA00ivqANXdqMLfAWqXp2uwKqCo2czYRVbOHC1bnPR6DV0xFF5b69RFixSulkPH2_C3Mu9PC5s5jKl1WKih96d9giVKl2E-sMo6gEb2-qVxMnI?key=LSSB9efPNxKCtrn5UlVYTA)


## 1. Create a Subscription Manifest:

- Log into Red Hat Customer Portal.

- Navigate to Subscriptions > Subscription Allocations > Create New subscription allocation.

- Name the manifest and select the appropriate version, e.g., Satellite 6.13.1

- Simple Content Access (SCA): Optional feature to simplify entitlement tasks, can be enabled for each organization.

- To add subscriptions: Subscription Allocations > Click manifest name > Subscriptions > Add Subscriptions.

- Set the desired number of entitlements for each product.


## 2. Export a Subscription Manifest:

- Go to Subscription Allocations > Click the manifest name.

- From the Subscriptions or Details tab, click Export Manifest.

- This encodes the subscription certificates into a compressed file for upload to Satellite Server.


## 3. Import a Subscription Manifest to Satellite Server:

- Ensure Satellite Server can synchronize with Red Hat CDN.

- In the Satellite web UI, set the organization context for the manifest.

- Navigate to Content > Subscriptions > Manage Manifest.

- Set Red Hat CDN URL (default: https\://cdn.redhat.com). For disconnected Satellites, specify your local content ISO location.

- In Subscription Manifest, click Browse to find and import the downloaded manifest.


# ![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXer3n2k6ncKwkmbSDDsXi7avP429UrbSULGutglWgAgrKgzSPyH6Df2idccSRK46jiStir898lHtc1y_hQJhyEvAXzUjILPJ5JWi-qP4irXp4XakHVasYcM6yf0ncw37gbei0D7nStJosHC1UXD205PaK5c?key=LSSB9efPNxKCtrn5UlVYTA)

# Software Lifecycles in Satellite Server

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXd8ceJ6Zzd8kmNpmWu0HjnUg_FMmM42Q1827JP-gjqXVEhGbI_OyUaomnFzE3Ls-AMigLGjsJAHMPXm4muxBYlTR5CC-pPHdSfcahM3-EkZNr5i88zbAJ5XpWCisXZ-NjGNZMNnw2_Yrs2JfY1GS8O5qPfi?key=LSSB9efPNxKCtrn5UlVYTA)


## 1. Application Lifecycle:

- Represents stages during software development and usage.

- A simple life cycle: Development -> Production.

- More complex life cycle: Development -> Testing -> Beta -> Production.

- In Satellite, these stages are named "environments".

- Each managed host is assigned one lifecycle environment based on its role (developer system, testing system, production host).


## 2. Environment Paths:

- Manage software package and errata releases using lifecycle environments.

- Sequence of stages is called an environment path.

- Every path starts with a "Library" environment, which syncs content from sources.

- A host in the Library environment has immediate access to the latest content.

Additional environments use promoted content views to define available content.


## Steps to Create a Lifecycle Environment Path:

- Set the right organization and location context.

- Go to Content > Lifecycle Environments > Create Environment Path.

- Input Name (auto-generates Label) and Description.

- Click Save.

\
\



## Steps to Extend an Environment Path:

- Set the right organization and location context.

- Go to Lifecycle Environment Paths > Add New Environment.

- Input Name and Description, then choose a prior environment.


## To Remove a Lifecycle Environment:

- Navigate to Lifecycle Environment Paths.

- Click on the environment name you want to delete.

- Click Remove Environment.


# Publish and Promote Content Views in Satellite Server

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcwWstSPFFpYo4--eI7QeDE85qLHPlhMWK8W_KAipDVOldsOKUICTgyLYyfslTY7Anr_eDomeN8td8zgee70Mf5OR6hISjoELlYPOqHZ8o09L4N8ZpPiGrmEzJFMHCRU6rZRgumYjFaY4AHlXnAtOiGDQs?key=LSSB9efPNxKCtrn5UlVYTA)


## 1. Content Views:

- Customized content repositories for specific environments.

- Can be filtered (e.g., package filters, errata filters).

- To Create: Navigate to Content > Content Views > Create New View. Input name, description, and save.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXd1Bid01g3vE2E2vV0yt6Dc1CUoQ_yUYGkVxGc2iBJzOv9mB4qsxXABrymQ9fgIaLQI2LIIJycdEYYn5Wg88M_F3_vFaEz7RXh0GGU5P8PPTX_uaAUShyZqZ3XtgL705V9Z4cQ4rTnhbbBFIOwPjF4LXvf_?key=LSSB9efPNxKCtrn5UlVYTA)


## 2. Managing Repositories in Content Views:

Initially, a new content view is empty.


## To Add:

- Go to Content > Content Views > select the content view.

- Click the Repositories tab.

- Check desired repositories > Add Repositories.

\
\



## To Remove:

- In the Repositories tab, check the desired repository.

- Click the vertical ellipsis menu > Remove.

\



# Publishing and Promoting Content Views in Satellite Server

## **Introduction**:

Content Views in Satellite Server provide a mechanism to define and curate the software that a specific environment should use. This mechanism enhances consistency and reliability across different stages in your infrastructure.


## 1. Publishing a Content View:

Purpose: Making a content view available in the library helps in versioning and tracking changes.

Versioning: Every time you publish or modify a content view, a sequentially numbered version is generated, allowing for easy tracking and rollback if necessary.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfqhPmobeZFlgUpTvDZywr_BEkPnIxahvNKctD7wERwqAtaNKZ2io_4p71bu-9yyCuLmV4NKTOiI8Qu8yDsWdPRDHexnOV4QgKwiQMJkViil6IQH-HUCZflBgAUNxiHLklBE8CMKOLWv3Wh3qlPlxm80qc?key=LSSB9efPNxKCtrn5UlVYTA)


## **Steps**:

- Navigate to Content > Content Views > select the desired content view.

- Implement desired updates (e.g., adding packages) and click Publish New Version.

- As the content version increments, it's advised to add a descriptive note for differentiation. Red Hat emphasizes the importance of a descriptive note to provide clarity on the differences between versions.

- After adding the description, click Next to review the changes. If satisfied, proceed by clicking Finish.


## 2. Promoting a Content View:

Purpose: Once the content view is published and ready, promoting it moves this version from the library to subsequent environments in the designated path. This allows for staging and incremental deployment.

Repository Management: For each environment, Satellite Server maintains separate repositories for each content view. When a content view gets promoted, associated repositories update to reflect the new content.


## Steps:

- Navigate to Content > Content Views > select the desired content view.

- In the Versions tab, find the published version you want to promote.

- Click on the vertical ellipsis menu at the end of the version row, then choose Promote.

- Add a detailed description to provide context on why this promotion is occurring.

* Finally, select the lifecycle environment to which you want to promote the content view, and click Promote.

\
\
\
\



# Managing Hosts with Host Collections in Red Hat Satellite 

Objective:

Organize registered hosts into host collections for concurrent management.

Host Collections Overview:

Host collections in Red Hat Satellite 6 allow grouping of content hosts with similar features. This aids in batch actions like package updates, errata installations, etc. Hosts can belong to multiple collections and can be grouped as per functions, department, or business units.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcbwN5jtFxpEZWK34SL47iKheLlu4ya21v0SCpguLtm0skJgjqhFmmIUB5KoFPuyYOdj1rArJ9ZeBUWosIpNV-tBa5G0b-2pubgfiqGaU3EIOyG66w0RScEobO1B8Jn565Hx4xnEMyIuAr6nfFBnKftXNk?key=LSSB9efPNxKCtrn5UlVYTA)

Steps to Manage Host Collections:

1. ## Creating a Host Collection:

- Go to Hosts > Host Collections.

- Click Create Host Collection.

- Provide a name and optional description.

- You have the option to limit the number of hosts in the collection by unchecking the Unlimited Hosts checkbox.

2. ## Managing Host Collection Membership:

- To add a host to a collection, it must first be registered with Satellite.

- Navigate to Hosts > Host Collections and select the desired host collection.

- In the Hosts tab, you can add or remove hosts.

3. ## Cloning a Host Collection:

- If you wish to replicate an existing collection, cloning can be useful.

- Select the host collection to clone from Hosts > Host Collections.

- From the Select Action list, choose Copy Host Collection and give the new collection a name.

4. ## Removing a Host Collection:

- Deletion of a host collection doesn't affect the member hosts' content or configuration. They just won't be part of that specific collection anymore.

- Navigate to Hosts > Host Collections and select the collection to delete.

- From the Select Action list, choose Remove. Confirm the deletion.

\



# Managing Custom Products and Repositories in Satellite Server 

\


Objective:

Create and manage custom repositories in Satellite Server for non-Red Hat content.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcyYcQofjLUAqeUaVlbJfLsnxTTuDvfB2SVRaAgGoIXcMiRAeOER23gjyi6Bif5wfRSYaoZG-MaLuSTaoGGqH-_5wbC_vEypddfz2h7jj9jZ6jnxTEmwgICDtiupTMUvoHwo90ondzPgIBQz3x4OU0ORgE?key=LSSB9efPNxKCtrn5UlVYTA)


## 1. Creating Custom Products:

- Navigate to Content > Products and select Create Product.

- Fill in the Name and Description.

- The Label field auto-populates from the Name.

- Optionally, attach a GPG Key to validate the origin of the packages in the product's repository.


## 2. Creating Custom Repositories:

- After creating a product, head to Content > Products.

- Click on the desired product name.

- Under the Repositories tab, select New Repository.

- Fill out the Name, Description, and select the repository type from the Type list. Additional fields will be made available based on the repository type selected.

- For yum repositories, a specific GPG key can be selected that will override the product-level GPG key.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXedDk3DuZlOuDRt3Q0Z1i_tZmszzQ_TFL18I-xjmPToYUmpSo_6Awaqc-85CcrtVo3pN_MHf6Y9D2d0p9VNWI7EUnjxFLsrBnLoig_tMNExs17q9EZiGS3A3fDlMBt0WPa8PMRUrjLCzCBGTC9Y_pC3cRrq?key=LSSB9efPNxKCtrn5UlVYTA)


## Key Repository Parameters:

- Restrict to Architecture: Specifies repository architecture.

- Upstream URL: Determines the source URL for the content.

- Upstream Username & Password: Credentials for protected upstream repositories.

- Download Policy: Determines if the content downloads immediately or on-demand.

- Verify SSL: Verifies upstream repository's SSL certificates.

- Mirror on Sync: Removes packages that are no longer in the upstream repository.

- Publish via HTTP: Allows packages to be accessible via HTTP.


## 3. Adding Packages to Repositories:

- Navigate to Content > Products and click on the desired product.

- In the Repositories tab, click the desired repository name.

- In the Upload Package section, choose the packages to upload and select Upload.

- You can view the uploaded packages in the Content Counts section.


## 4. Removing Packages from Repositories:

- Navigate to the Packages page of your custom product repository.

- Choose the packages you wish to delete and click Remove Packages.
