Clone of https://code.google.com/p/php-mempeak/

# Usage

    ./php_mempeak_munin setup

# About

Ostensibly a munin plugin, php_mempeak is turning into a stand-alone application.

On exit, php scripts check if their memory usage (see memory_get_peak_usage) was above a given threshold, and dump a brief log if so. php_mempeak runs as a cron job (called by munin) and stores the log with the highest byte count since the last run in a round robin database. Munin shows us this data as a pretty graph.

It came about when I got an SOS from a client who OOMed on a VPS that had until now been behaving normally. He has a few Wordpress sites so I instinctively assumed that codebase to be the smoking gun. However, he had no monitoring software on the machine at the time so having got an opcode cache in place I installed Munin so that we could at least get some more detailed info if it happened again.

However, Munin doesn't seem to yet have a plugin that can give you detailed information about which of your virtualhost domains' scripts are asking for the most memory, so I wrote one.

The idea behind php_mempeak is not just to draw pretty graphs that tell you which virtualhost asks for the most memory per-request, but to store more comprehensive data for a little while, as efficiently as possible, in order to later be able to query the database about what was happening over a given recent time period for a certain domain. This way we can look at the uris that were visited during an OOM event and in conjunction with apache logs see whether the event was caused by unusually heavy traffic, by bloated, badly-written scripts, or some other issue.

