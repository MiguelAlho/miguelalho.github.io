---
title: Open Telemetry Notes
date: 2023-04-19T12:10:27.7875855+01:00
author: Miguel Alho
draft: false
url: /bookmarks/Open-Telemetry-Notes
imageFolderId: 1098203960365285376
article:
  author:
    name: ''
    twitter: ''
    linkedIn: ''
    website: ''
  links:
  - https://github.com/open-telemetry/opentelemetry-collector/blob/main/docs/design.md
  - https://opensearch.org/blog/distributed-tracing-pipeline-with-opentelemetry/
  - https://opentelemetry.io/blog/2022/tail-sampling/
  - https://github.com/open-telemetry/opentelemetry-collector-contrib/blob/main/exporter/loadbalancingexporter/README.md
  - https://github.com/open-telemetry/opentelemetry-collector-contrib/blob/main/processor/tailsamplingprocessor/README.md
  - https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/processor/probabilisticsamplerprocessor
  - https://opentelemetry.io/docs/collector/scaling/
video:
  source: youtube
  id: ''
  link: ''
  author:
    name: ''
    twitter: ''
    linkedIn: ''
    website: ''
  context:
    name: ''
    link: ''
tags: []
summary: ''
notes:
- id: 0
  type: Slide
  time: ''
  image: 1098203960365285376/1098204374288564224.png
  title: Architecture
  content: >-
    Data receiving, processing, and exporting is done using Pipelines. The Collector can be configured to have one or more pipelines. Each pipeline includes:


    * a set of Receivers that receive the data

    * a series of optional Processors that get the data from receivers and process it

    * a set of Exporters which get the data from processors and send it further outside the Collector.


    The same receiver can be included in multiple pipelines and multiple pipelines can include the same Exporter.


    There can be one or more receivers in a pipeline. Data from all receivers is pushed to the first processor, which performs a processing on it and then pushes it to the next processor (or it may drop the data, e.g. if it is a “sampling” processor) and so on until the last processor in the pipeline pushes the data to the exporters. Each exporter gets a copy of each data element. The last processor uses a fanoutconsumer to fan out the data to multiple exporters.


    A pipeline configuration typically looks like this:


    ```

    service:
      pipelines: # section that can contain multiple subsections, one per pipeline
        traces:  # type of the pipeline
          receivers: [otlp, jaeger, zipkin]
          processors: [memory_limiter, batch]
          exporters: [otlp, jaeger, zipkin]
    ```
  comment: ''
- id: 1
  type: Slide
  time: ''
  image: 1098203960365285376/1098205458079940608.png
  title: Receivers
  content: >-
    A receiver that is a part of multiple pipelines will have fan out element after it, to forward the events to multiple processors.


    If one of the processers following the fan out blocks, the receiver will be blocked, and block on ALL pipelines.
  comment: ''
- id: 2
  type: Note
  time: ''
  image: ''
  title: Exporters
  content: >
    * The configuration allows to have multiple exporters of the same type, even in the same pipeline.

    * Usually an exporter gets the data from one pipeline, however it is possible to configure multiple pipelines to send data to the same exporter
  comment: ''
- id: 3
  type: Slide
  time: ''
  image: ''
  title: Processors
  content: >2-
     pipeline can contain sequentially connected processors. The first processor gets the data from one or more receivers that are configured for the pipeline, the last processor sends the data to one or more exporters that are configured for the pipeline. All processors between the first and last receive the data strictly only from one preceding processor and send data strictly only to the succeeding processor.

    Processors can transform the data before forwarding it (i.e. add or remove attributes from spans), they can drop the data simply by deciding not to forward it (this is for example how the `probabilisticsampler` processor works), they can also generate new data. This is how a `spanmetrics` processor can produce metrics for spans processed by the pipeline.


    The same name of the processor can be referenced in the processors key of multiple pipelines. In this case the same configuration will be used for each of these processors however each pipeline will always get its own instance of the processor. Each of these processors will have its own state, the processors are never shared between pipelines. 


    The same name of the processor MUST NOT be referenced multiple times in the processors key of a single pipeline.
  comment: ''
- id: 4
  type: Note
  time: ''
  image: 1098203960365285376/1098207321638567936.png
  title: Running as an Agent
  content: 'you can run OpenTelemetry Collector as an Agent. The Agent runs as a daemon in the VM/container and can be deployed independent of Library. Once Agent is deployed and running, it should be able to retrieve traces/metrics/logs from Library, export them to other backends. '
  comment: ''
- id: 5
  type: Note
  time: ''
  image: 1098203960365285376/1098207677734977536.png
  title: Running as a Gateway
  content: The OpenTelemetry Collector can run as a Gateway instance and receives spans and metrics exported by one or more Agents or Libraries, or by tasks/agents that emit in one of the supported protocols. The Collector is configured to send data to the configured exporter(s).
  comment: ''
- id: 6
  type: Note
  time: ''
  image: ''
  title: Processing Use Cases
  content: >
    ### Telemetry mutation


    Some types of mutation include


    - Remove a forbidden attribute such as `http.request.header.authorization`

    - Reduce cardinality of an attribute such as translating `http.target` value of `/user/123451/profile` to `/user/{userId}/profile`

    - Decrease the size of the telemetry payload by removing large resource attributes such as `process.command_line`

    - Filtering out signals such as by removing all telemetry with a `http.target` of `/health`

    - Attach information from resource into telemetry, for example adding certain resource fields as metric dimensions


    The processors implementing this use case are `attributesprocessor`, `filterprocessor`, `metricstransformprocessor`, `resourceprocessor`, `spanprocessor`.


    ### Metric generation


    The collector may generate new metrics based on incoming telemetry.


    - Create new metrics based on information in spans, for example to create a duration metric that is not implemented in the SDK yet

    - Apply arithmetic between multiple incoming metrics to produce an output one, for example divide an `amount` and a `capacity` to create a `utilization` metric


    The processors implementing this use case are `metricsgenerationprocessor`, `spanmetricsprocessor`.


    ### Grouping


    Some processors are stateful, grouping telemetry over a window of time based on either a trace ID or an attribute value, or just general batching.


    - Batch incoming telemetry before sending to exporters to reduce export requests

    - Group spans by trace ID to allow doing tail sampling

    - Group telemetry for the same path


    The processors implementing this use case are `batchprocessor`, `groupbyattrprocessor`, `groupbytraceprocessor`.



    ### Telemetry enrichment


    commonly used to fill gaps in coverage of environment specific data.


    - Add environment about a cloud provider to `Resource` of all incoming telemetry


    The processors implementing this use case are `k8sattributesprocessor`,  `resourcedetectionprocessor`.
  comment: Grouping bit is interesting for tail sampling
- id: 7
  type: Note
  time: ''
  image: ''
  title: ''
  content: Transformation language examples at https://github.com/open-telemetry/opentelemetry-collector/blob/main/docs/processing.md#examples
  comment: ''
- id: 8
  type: Note
  time: ''
  image: ''
  title: AWS X-Ray Tracing Exporter Data Conversion
  content: >+
    This exporter converts OpenTelemetry spans to AWS X-Ray Segment Documents and then sends them directly to X-Ray using the PutTraceSegments API.


    Trace IDs and Span IDs are expected to be originally generated by either AWS API Gateway or AWS ALB and propagated by them using the X-Amzn-Trace-Id HTTP header. However, other generation sources are supported by replacing fully-random Trace IDs with X-Ray formatted Trace IDs where necessary:


    > AWS X-Ray IDs are the same size as W3C Trace Context IDs but differ in that the first 32 bits of a Trace ID is the Unix epoch time when the trace was started. Since X-Ray only allows submission of Trace IDs from the past 30 days, received Trace IDs are checked and spans without a valid timestamp are dropped.


    AWS X-Ray can be integrated with CloudWatch Logs to correlate traces with logs. For this integration to work, the X-Ray segments must have the AWS Property cloudwatch_logs set. 

  comment: ''
- id: 9
  type: Slide
  time: ''
  image: 1098203960365285376/1098213204389003264.png
  title: Deployment methods
  content: >+
    The two primary deployment methods:


    * Agent: A collector instance running with the application or on the same host as the application (e.g. binary, sidecar, or daemonset).

    * Gateway: One or more collector instances running as a standalone service (e.g. container or deployment) typically per cluster, data center or region.


    The OpenTelemetry agent forwards the telemetry data to an OpenTelemetry collector gateway. A Gateway cluster runs as a standalone service and offers advanced capabilities over the agent, including tail-based sampling. In addition, a Gateway cluster can limit the number of egress points required to send data, as well as consolidate API token management.


  comment: https://opensearch.org/blog/distributed-tracing-pipeline-with-opentelemetry/
- id: 10
  type: Note
  time: ''
  image: ''
  title: ''
  content: >-
    Tail Sampling Processor enables us to make more intelligent choices when it comes to sampling traces. This is especially true for latency measurements, which can only be measured after they are complete. Since the collector sits at the end of the pipeline and has a complete picture of the distributed trace, sampling determinations are made in opentelemetry collector to sample based on isolated, independent portions of the trace data.


    Today, this processor only works with a single instance of the collector. The workaround is to utilize Trace ID aware load balancing to support multiple collector instances and avoid a single point of failure. This load balancer exporter is added to the agent configuration. It is responsible for consistently exporting spans and logs belonging to a trace to the same backend gateway collector for tail based sampling.


    The filters for tail based sampling can be chained together to get the desired effect:


    * latency: Sample based on the duration of the trace. The duration is determined by looking at the earliest start time and latest end time, without taking into consideration what happened in between.

    * probabilistic: Sample a percentage of traces.

    * status_code: Sample based upon the status code (OK, ERROR or UNSET)

    * rate_limiting: Sample based on rate of spans per trace per second

    * string_attribute: Sample based on string attributes value matches, both exact and regex value matches are supported
  comment: ''
- id: 11
  type: Slide
  time: ''
  image: 1098203960365285376/1098258841822298112.png
  title: ''
  content: >
    Reasons for sampling:


    * Manage costs: 

    * Focus on interesting traces: 

    * Filter out noise


    Tail-based sampling is where the decision to sample a trace happens after all the spans in a request have been completed. This is in contrast to head-based sampling, where the decision is made at the beginning of a request when the root span begins processing. Tail-based sampling gives you the option to filter your traces based on specific criteria, which isn’t an option with head-based sampling.


    To use tail sampling in OpenTelemetry, you need to implement a component called the `tail sampling processor`. This component samples traces based on a set of policies that you can choose from and define. First, to ensure that you’re capturing all spans, use either the default sampler or the AlwaysOn sampler in your SDKs.
  comment: https://opentelemetry.io/blog/2022/tail-sampling/
- id: 12
  type: Slide
  time: ''
  image: 1098203960365285376/1098259675121451008.png
  title: ''
  content: ''
  comment: ''
- id: 13
  type: Note
  time: ''
  image: ''
  title: Limitations
  content: >2
     Furthermore, for tail sampling to work, all the spans of a particular trace have to be processed in the same collector, which leads to scalability challenges.

    For a simple setup, one collector will suffice, and no load balancing is needed. However, the more requests that are being held in memory, the more memory you’ll need, and you’ll also need additional processing and computing power to look at each span attribute. As your system grows, you can’t do all this with just a single collector, which means you have to think about your collector deployment pattern and load balancing.


    Since one collector is insufficient, you have to implement a two-layer setup in which the collector is deployed in an agent-collector configuration. You also need each collector to have a full view of the traces it receives. This means that all spans with the same trace ID need to go to the same collector instance, or you’ll end up with fragmented traces.


    Also, you’re only getting a look at the measurements of sampled data, not accurate measurements in regards to all data.
  comment: ''
- id: 14
  type: Note
  time: ''
  image: ''
  title: Trace ID/Service-name aware load-balancing exporter
  content: >+
    This is an exporter that will consistently export spans and logs depending on the routing_key configured. If no routing_key is configured, the default routing mechanism is traceID. This means that spans belonging to the same traceID (or service.name, when service is used as the routing_key) will be sent to the same backend.

  comment: https://github.com/open-telemetry/opentelemetry-collector-contrib/blob/main/exporter/loadbalancingexporter/README.md
- id: 15
  type: Note
  time: ''
  image: ''
  title: Tail Sampling Processor
  content: >-
    The tail sampling processor samples traces based on a set of defined policies. All spans for a given trace MUST be received by the same collector instance for effective sampling decisions.


    This processor requires all spans for a given trace to be sent to the same collector instance for the correct sampling decision to be derived. When scaling the collector, you'll then need to ensure that all spans for the same trace are reaching the same collector. You can achieve this by having two layers of collectors in your infrastructure: one with the load balancing exporter, and one with the tail sampling processor.


    While it's technically possible to have one layer of collectors with two pipelines on each instance, we recommend separating the layers in order to have better failure isolation.
  comment: https://github.com/open-telemetry/opentelemetry-collector-contrib/blob/main/processor/tailsamplingprocessor/README.md
- id: 16
  type: Note
  time: ''
  image: ''
  title: Probabilistic vs tail
  content: >-
    The probabilistic sampling processor and the probabilistic tail sampling processor policy work very similar: based upon a configurable sampling percentage they will sample a fixed ratio of received traces. But depending on the overall processing pipeline you should prefer using one over the other.


    As a rule of thumb, if you want to add probabilistic sampling and...


    ...you are not using the tail sampling processor already: use the probabilistic sampling processor. Running the probabilistic sampling processor is more efficient than the tail sampling processor. The probabilistic sampling policy makes decision based upon the trace ID, so waiting until more spans have arrived will not influence its decision.


    ...you are already using the tail sampling processor: add the probabilistic sampling policy. You are already incurring the cost of running the tail sampling processor, adding the probabilistic policy will be negligible. Additionally, using the policy within the tail sampling processor will ensure traces that are sampled by other policies will not be dropped.
  comment: ''
- id: 17
  type: Note
  time: ''
  image: ''
  title: Probabilistic Sampling Processor
  content: >-
    The probabilistic sampler supports two types of sampling for traces:


    * `sampling.priority` semantic convention as defined by OpenTracing

    * Trace ID hashing


    Config options:


    * `hash_seed`(no default): An integer used to compute the hash algorithm. Note that all collectors for a given tier (e.g. behind the same load balancer) should have the same hash_seed.

    * `sampling_percentage` (default = 0): Percentage at which traces are sampled; >= 100 samples all traces
  comment: https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/processor/probabilisticsamplerprocessor
- id: 18
  type: Note
  time: ''
  image: ''
  title: Scaling
  content: >+
    Regarding scaling, we have three types of components: stateless, scrapers, and stateful.


    Most Collector components are stateless. Even if they hold some state in memory, it isn’t relevant for scaling purposes.


    Scrapers, like the Prometheus receiver, are configured to obtain telemetry data from external locations. The receiver will then scrape target by target, putting data into the pipeline.


    Components like the tail sampling processor cannot be easily scaled, as they keep some relevant state in memory for their business. Those components require some careful consideration before being scaled up.

  comment: https://opentelemetry.io/docs/collector/scaling/
- id: 19
  type: Note
  time: ''
  image: ''
  title: Scaling Stateless Collectors
  content: >-
    Most of the time, scaling the Collector is easy, as it’s just a matter of adding new replicas and using an off-the-shelf load balancer. 


    You should still consider splitting your collection pipeline with reliability in mind. For instance, when your workloads run on Kubernetes, you might want to use DaemonSets to have a Collector on the same physical node as your workloads and a remote central Collector responsible for pre-processing the data before sending the data to the storage. When the number of nodes is low and the number of pods is high, Sidecars might make more sense, as you’ll get a better load balancing for the gRPC connections among Collector layers without needing a gRPC-specific load balancer. Using a Sidecar also makes sense to avoid bringing down a crucial component for all pods in a node when one DaemonSet pod fails.
  comment: ''
- id: 20
  type: Note
  time: ''
  image: ''
  title: Scaling Stateful Collectors
  content: "It is the case for the tail-sampling processor, which holds spans in memory for a given period, evaluating the sampling decision only when the trace is considered complete. Scaling a Collector cluster by adding more replicas means that different collectors will receive spans for a given trace, causing each collector to evaluate whether that trace should be sampled, potentially coming to different answers. This behavior results in traces missing spans, misrepresenting what happened in that transaction.\n\nA similar situation happens when using the span-to-metrics processor to generate service metrics. When different collectors receive data related to the same service, aggregations based on the service name will be inaccurate.\n\nTo overcome this, you can deploy a layer of Collectors containing the load-balancing exporter in front of your Collectors doing the tail-sampling or the span-to-metrics processing. The load-balancing exporter will hash the trace ID or the service name consistently and determine which collector backend should receive spans for that trace. "
  comment: ''
content: ''

---
