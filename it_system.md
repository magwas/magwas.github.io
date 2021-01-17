# My priorities for an IT system

## Configuration management

* All configuration comes from a central place
 * The system can be autmatically deployed (using ansible or similar tool)
 * The configuration is under version control (e.g. git)
 * The origin of truth is a model

## Key management

* No shared secrets
 * All secrets are separated to its own filesystem (/keys)
 * No passwords (passwordless sudo, cert based auth for services like database)
 * Secrets can be redeployed by a script
 * Use certbot for public certs
 * The stuff which cannot use public certs should be based on an offline CA
 * in-place key generation

## Subsystem separation and information flow control
* Separation: all subsystems should ideally have their own vm or docker container. The separation method should be uniform
* information flow control: operations, backup, and log management should be controlled from a well-protected subsystem, which have access to all others, but no other subsystem  have access to it

## data management and backup

* dynamic volume management (LVM)
 * for large-volume low-bandwith data (e.g. architecture repo) use a service providing cheap storage (rsync)
* backup
 * configuration and non-reconstructable data should be backed up
 * the volume of backup should be kept minimal
  * no general fs backup
  * no backup of generated files (including the architecture repository)
 * make sure that anything can be rebuilt from backup

## audit

* system log
 * all log should be handled by syslog-ng, including web servers, application servers and databases
 * all subsystems should have a /var/log/messages with all its messages, and /dev/xconsole pipe for all the messages
 * all logs should be sent to a central place to enable forensic analysis and realtime log analysis (out of scope for now)

## operation management

* There should be a central place where the health of the system can be quicly overviewed (like nagios)
* All services should be monitored, and SLA-relevant information should be gathered and displayed
* If a service does not work as excepted, alert should be sent to the appropriate personnel (though email or discord)

## Service Level Agreement

* All services should have an agreed upon SLA, and a monthly fee to operate
* The SLA should include at least the availability and the problem solving time
* A ticketing system should track the problems and feature requests
 * problems should be solved according to the SLA
 * for feature requests the price and timeframe are agreed on for each individual ticket
 * for returning cofiguration changes (e.g. creating a new user), the price and timing for the kind of request is defined in the service SLA
