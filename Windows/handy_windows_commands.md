# Handy Commands

## PowerShell Env Manipulation

- set path=%path%;[location/to/add/to/path]
- get-executionpolicy - see the current execution level
- new-alias [alias] [command it is replacing]
    - Add it to proile - `"New-Alias which get-command" | add-content $profile`


### Execution Policy  

1. Restricted (default behavior)
2. All Signed
    - If the execution policy is set to All Signed then scripts will be allowed to run, but only if they are signed by a trusted publisher.
3. Remote Signed
    -  If the execution policy is set to Remote Signed, any PowerShell scripts that have been locally created will be allowed to run. Scripts created remotely are allowed to run only if they are signed by a trusted publisher.
4. Unrestricted (Wheeeeeeeeeeee!)

- set-executionpolicy -scope currentuser
  - Unrestricted (to run PowerShell as admin)

##  Process Manipulation

- Stopping a process

  `stop-process -name [process name]`  
  `stop-process -id [number]`  

- get-service - list services
- get-process - lists running processes

## Event Logs

- get-eventlog -log "application"

## Installing Windows Components

- Install-WindowsFeature rsat-adds - this can only be used on Windows Server.

## USER & GROUP Manipulation

`net localgroup [group name] [Domain\username] /add`

## Mounting network shares via cmd

`net use <driveletter>: \\<server>\<sharename> /USER:<domain>\<username> <password> /PERSISTENT:YES`

## Getting Product UUIDs

`get-wmicobject -class win32_product` 

`get-ciminstance -classname win32_product | select name, identifyingnumber`


## Install Chocolatey

Run PowerShell as admin the run the following commands

    > `set-executionpolicy -scope currentuser Unrestricted`

### Download Chocolatey

    > iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))


### Install Git

    > choco install git.install -params '"/GitOnlyOnPath"' -y

### Install Python 3

    > choco install python -y

Add `pip` to path

    > [Environment]::SetEnvironmentVariable("Path", "$env:Path;C:\tools\python\Scripts\", "User")

## Misc. Commands

- export-csv - pipe another command to this
- select-object Name, Status, etc - narrow down the output