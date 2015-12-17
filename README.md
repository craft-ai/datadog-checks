# Stripe Datadog checks

This is a collection of plugins — [checks](http://docs.datadoghq.com/guides/agent_checks/) in Datadog parlance — for the [Datadog agent](https://github.com/datadog/dd-agent) that Stripe has found useful with Datadog.

# Motivation

We've sent a lot of patches to [Datadog](https://www.datadoghq.com/) and we regularly work closely with them on our ideas. But sometimes we want something that isn't a fit for the mainline Datadog agent. To that end we've created this repository to hold work that is either in flight or was decided to not be a fit for inclusion in the core agent set. We hope you find it useful!

# Using The Checks

Place the `.py` file you want to use in to the checks directory — `/etc/dd-agent/checks.d` by default — and the YAML config file in the config directory — `/etc/dd-agent/conf.d` by default — and you should be ready to do! Restart the agent and run `/etc/init.d/datadog-agent info` to verify that the plugin is working.

Each plugin here is provided with a sample config file containing some documentation.

# Checks

Here's our list of checks!

## Linux Proc Extras

There are some additional Linux metrics we like to watch that aren't included in the core Datadog agent.
This check adds the following metrics from Linux's `/proc` filesystem:

* inode information
  * `system.inodes.total`
  * `system.inodes.used`
* entropy available
  * `system.entropy.available`
* context switch count (for rates!)
  * `linux.context_switches`
* processes created count (for rates!)
  * `linux.processes_created`
* interrupts
  * `linux.interrupts`
* counts of processes by state, with tags
  * `system.processes.states`
    * `state:uninterruptible`
    * `state:runnable`
    * `state:sleeping`
    * `state:stopped`
    * `state:paging`
    * `state:dead`
    * `state:zombie`
* counts of processes by priority, with tags
  * `system.processes.priorities`
    * `priority:low`
    * `priority:high`
    * `priority:locked`

## Nagios Runner

The Nagios Runner check takes a list of check "instances". The instances are each executed and, [according to the Nagios Plugin API](https://assets.nagios.com/downloads/nagioscore/docs/nagioscore/3/en/pluginapi.html) the return value is inspected and a [service check](http://docs.datadoghq.com/api/#service_checks) is submitted using the provided name.

**Note**: The checks supplied are executed sequentially. You may run in to performance issues if you attempt to run too many checks or checks that execute very slowly. This will effectively block the agent and cause all sorts of hiccups!

```yaml
init_config:
  # Not needed

instances:
  - name: "stripe.check.is_llama_on_rocket"
    command: "/usr/lib/nagios/plugins/check_if_llama_is --on rocket"
  - name: "stripe.check.falafel_length"
    command: "/usr/lib/nagios/plugins/check_falafel -l 1234"
```
