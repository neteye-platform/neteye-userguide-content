
By default only Beats and Elastic Agents events are sent to El Proxy, if enabled.
NetEye, however, provides an output also in the main pipeline to
redirect events to El Proxy.

Logstash sends logs to El Proxy when field ``[EBP_METADATA][event][module]`` is set to ``elproxysigned``,
with the logs being redirected to the `ebp_persistent` pipeline.

For example, to send all ``syslog`` logs to El Proxy, you can use a filter similar to the
following ::

  filter {
  if [type] == "syslog" {
    if [EBP_METADATA][event][module] {
      mutate {
        replace => {"[EBP_METADATA][event][module]" => "elproxysigned"}
      }
    } else {
        mutate {
          add_field => {"[EBP_METADATA][event][module]" => "elproxysigned"}
        }
      }
    }
  }

All logs passed through the `ebp_persistent` pipeline will be disk-persisted.
