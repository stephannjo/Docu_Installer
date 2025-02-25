# Configuration of the POS Application

This document outlines the configuration files used by the POS application, detailing their purpose and parameters related to RabbitMQ and initial installation.

## 1. Configuration Files

### 1.1 RabbitMQ Configuration Files
- **jifs-prj.properties**: Contains project-specific RabbitMQ user credentials for upstream events, downstream data, control, and software queues.
- **jifs-prj2_install.properties**: Specifies settings for consuming from various queues, including enabling SSL and trusting all certificates for RabbitMQ connections.
- **jifs-prjNames.properties**: Contains configuration settings for consuming from different RabbitMQ queues and heartbeat endpoints.
- **rabbitmq.endnode.config**: Defines RabbitMQ shovel configurations for transferring messages between different brokers and queues.

```plaintext
├── home
│   └── kasse
│       ├── fjifs
│       │   ├── bin
│       │   │   ├── jifs-prj.properties
│       │   │   ├── jifs-prj2_install.properties
│       │   │   └── jifs-prjNames.properties
│       └── rmq
│           └── rabbitmq.endnode.config

### 1.2 SQLite Configuration File
- **inst_customize.sqlite**: Contains configuration data for the POS application. The file is located at `C:\eurosuite\PosSetupZalando\resources\main\data\inst_customize.sqlite`.

## 2. SQLite File Structure

### 2.1 Definition of Fields in `inst_customize.sqlite`

**Note**: The examples show possible inputs. When entering predefined selection options, the spelling (case sensitive) must be observed. Umlauts (umlauts) are not allowed in the designations, such as location and street.

Unnecessary fields may be completely removed from the `inst_customize.sqlite`.

### 2.2 Tables and Fields

#### Table: `basisdata`

| Field       | Description                        | Example | Notes                                                                 |
|-------------|------------------------------------|---------|-----------------------------------------------------------------------|
| storeid     | Store number of the branch         | 6       | Corresponds to the store number in euroCONTROL                        |
| posid       | Cash register number               | 1       | Corresponds to the cash register number of the respective branch in euroCONTROL |
| ci_type     | Cash register type                 | pos     | always pos                                                            |
| concentrator| Concentrator cash register yes/no  | true    | Concentrator cash register = true, satellite cash registers = false   |

#### Table: `generaldata`

| Field           | Description                        | Example           | Notes                                                                 |
|-----------------|------------------------------------|-------------------|-----------------------------------------------------------------------|
| storeid         | Store number of the branch         | 6                 | Corresponds to the store number in euroCONTROL                        |
| store_name1     | Name1 of the branch                | Branch Hamburg    | Name of the branch, serves as a guide and is not mandatory            |
| store_name2     | Name2 of the branch                | Branch Hamburg    | Name of the branch, serves as a guide and is not mandatory            |
| store_street    | Street of the branch               | Große Bleichen    | Street of the branch, serves as a guide and is not mandatory          |
| store_city      | City of the branch                 | Hamburg           | City of the branch, serves as a guide and is not mandatory            |
| store_zip       | ZIP code of the branch             | 20354             | ZIP code of the branch, serves as a guide and is not mandatory        |
| store_country   | Country of the branch              | DE                | Country code of the branch, serves as a guide and is not mandatory    |
| clientid_country| Country-Channel of the branch      | 10100             | Can be found in the organizational structure in euroCONTROL           |
| clientid_channel| Client-Channel of the branch       | 100               | Can be found in the organizational structure in euroCONTROL           |
| store_typ       | Operating environment of the branch| test              | Test system = test, production system = prod                          |

#### Table: `instdata`

| Field       | Description                        | Example                   | Notes                                                                 |
|-------------|------------------------------------|---------------------------|-----------------------------------------------------------------------|
| ID          | key field                          | 1                         | Automatically assigned                                                |
| actualAddr  | IP-euroCONTROL                     | 10.10.10.40               | IP from euroCONTROL                                                   |
| targetDir   | Path of the cash register installation | c:\home\kasse         | Path where the cash register should be installed                      |
| cygwinDir   | Path for cygwin64                  | c:/eurosuite/cygwin64/bin | Path where the cygwin64 tool is located                               |

#### Table: `macdata`

| Field       | Description                        | Example               | Notes                                                                 |
|-------------|------------------------------------|-----------------------|-----------------------------------------------------------------------|
| macid       | MAC address of the cash register   | AE:DE:48:02:01:00     | MAC address in Linux format                                           |
| storeid     | Store number of the branch         | 6                     | Corresponds to the store number in euroCONTROL                        |
| posid       | Cash register number               | 1                     | Corresponds to the cash register number of the respective branch in euroCONTROL |

## 3. Editing Configuration Files

To edit the configuration files:
1. Open the file in a text editor.
2. Make the necessary changes.
3. Save the file.

**Caution**: Manual edits should be made with caution and only if you understand the implications of the changes.