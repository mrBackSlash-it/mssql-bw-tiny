# mssql-bw-tiny

## What?
a slightly modified MSSQL BitWarden docker image modified to allow execution on a machine with less than 2 GiB of physical memory

## Why?
Microsoft's sqlservr, at startup, looks to see how much physical memory its host has
if the host has less than 2 GiB sqlservr just quits with the message:
"sqlservr: This program requires a machine with at least 2000 megabytes of memory."
it turns out that sqlservr does not try to allocate that much memory and fail but rather sqlservr enforces that as a policy (perhaps to cut down on performance related issues due to page faulting)
but i think if someone wants to use swap space instead of physical memory that is up to that person


## How?
using LD_PRELOAD to redefine the system call "sysinfo"
when sqlservr invokes the "sysinfo" system call (to see how much physical memory the host has) we instead lie to sqlservr with our redefined sysinfo function


## How do i use this?
- in the folder where you have your bitwarden data navigate to the `docker` folder
- add the file `docker-compose.override.yml` next to the original `docker-compose.yml`
- restart your bitwarden instance


---

- `https://hub.docker.com/_/microsoft-mssql-server`
- `https://github.com/Microsoft/mssql-docker`
- `https://github.com/justin2004/mssql_server_tiny`


