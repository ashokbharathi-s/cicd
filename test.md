<H1 align="center">Dealer search service - SLO objectives</H1>
<table>
  <tr>
    <td>Author</td>
    <td>Sarankumar Chandrasekaran</td>
  </tr>
  <tr>
    <td>SLO Adopation Leader</td>
    <td>Balachandar Natarajan</td>
  </tr>
  <tr>
    <td>SLI/SLO Owner</td>
    <td>Saiprasad Potluri/Ratna Puttaswamy</td>
  </tr>
  <tr>
    <td>SLI/SLO Approvers</td>
    <td>Muthupalaniappan V/Mathivanan V </td>
  </tr>
  <tr>
    <td>Approval Date </td>
    <td>10/09/2023</td>
  </tr>
  <tr>
    <td>Revisit Date </td>
    <td>01/09/2024</td>
  </tr>
  <tr>
    <td>Approval Date </td>
    <td>10/09/2023</td>
  </tr>
  <tr>
    <td>Ford Github URL</td>
    <td>To be included</td>
  </tr>
  <tr>
    <td>API Catalog URL</td>
    <td>API Catalog URL for Dealer Search</td>
  </tr>
</table>

# **Service Overview**

**Dealer Search service** is designed to help users find dealers who
have Ford catalog vehicles for sale within a specified radius of their
location. To use the service, the user must provide their location
coordinates (latitude and longitude) along with a maximum search radius
and an optional catalog id. This service can be particularly useful for
customers who are interested in buying a Ford vehicle but are unsure
which dealers in their area have them in stock. The V1 version of this
service is primarily intended for the North American market while the V4
is for IMG and V5 for EU markets.

**AutoSelectDealer service** is a Ford dealer search tool that allows
users to find dealers in the IMG Market who sell a specific Ford vehicle
based on a catalog ID and location coordinates. Additionally, the
request includes a set of vehicle features for which the user is seeking
pricing information. The response includes detailed pricing information
for the specified vehicle based on the selected features.

**AZ pin service** is an API that allows users to validate their AXZ PIN
(Personal Identification Number) for a specific program or offer related
to Ford vehicles in the USA market. The request includes a unique AXZ
PIN code in the format \"X-50-097-57\", which is used to verify the
user\'s eligibility for a specific program or offer.

**DeliveryOptionsV1 service** allows users to get details of delivery
options provided by Ford dealers in the EU market. The request includes
the user\'s delivery location information, including the delivery ZIP
code, latitude, longitude, and delivery country (in ISO code format).
The response includes detailed information about the delivery options
available for the specified dealer, including the delivery methods
(e.g., collection, home delivery), the associated delivery charges (if
any), and whether the delivery is international.

**Data** **Source**: All SLI's mentioned in this document are observed
and measured from the individual microservice's API access log (from the
Google Cloud Run layer)

**Compliance** **window**: All the service level objectives use a
four-week (28 days) rolling window.

# **SLI/SLO: Availability**

**Objective**: If users receive errors, incorrect data, or no data at
all, the service is functionally unavailable to them. The SLO for
availability aims to ensure that the service is functioning correctly
and delivering the intended results to users.

**SLI**: The proportion of successful requests, as measured from the
application access log metrics. Any HTTP status other than 500--599 is
considered successful.

**SLI Calculation:** *count of \"api\" http_requests which do not have a
5XX status code divided by count of all \"api\" http_requests, on a
rolling 28-day window.*

**SLO**: Percentage of successful requests. For example, an SLO of 99.5%
means that at least 995 out of 1000 requests should have a non-500
response code for a rolling window of 28 days.
<table>
  <tr>
    <td>DealerSearchV1</td>
    <td>99.5% Success</td>
  </tr>
  <tr>
    <td>DealerSearchV4</td>
    <td>99.5% Success</td>
  </tr>
  <tr>
    <td>DealerSearchV5</td>
    <td>99.5% Success</td>
  </tr>
  <tr>
    <td>AutoSelectDealer</td>
    <td>99.5% Success</td>
  </tr>
  <tr>
    <td>LeadTimeSearch</td>
    <td>99.5% Success</td>
  </tr>
</table>

# **SLI/SLO: Latency**

**Objective**: The objective is to develop a system that is not only
highly available but also performs effectively. Even if a system is
available most of the time, if it is sluggish, users will still perceive
it as unreliable. When users are required to wait for an unreasonable
amount of time to receive service, it is referred to as latency.
Therefore, it is important to prioritize both availability and
performance to ensure that the system meets user expectations.

**SLI:** The proportion of sufficiently fast requests, as measured from
the application access log metrics. "Sufficiently fast" indicates the
percentage of requests that meet the performance criteria defined by the
90th and 99th percentiles of all requests served in the given
measurement window.

**SLI Calculation:**

**90th Percentile measurement:** *count of number of \"api\" HTTP
requests that have a duration less than or equal to a specified time of
\"XX\" milliseconds and divide that by the count of all \"api\" HTTP
requests \>= 90%*

**99th Percentile measurement:** *count of number of \"api\" HTTP
requests that have a duration less than or equal to a specified time of
\"XX\" milliseconds and divide that by the count of all \"api\" HTTP
requests \>= 99%.*

**SLO**: This tiered SLO is defined for a 28-day single-window
measurement. This means that all the requests received in the entire
28-day compliance window are treated as one bucket. There are no
multi-window-based measurements.


+----------------+-----------------------------------------------------+
| DealerSearchV1 | 90 % of requests \< 100 ms                          |
|                |                                                     |
|                | 99% of requests \< 275 ms                           |
+================+=====================================================+
| DealerSearchV4 | 90 % of requests \< 75 ms                           |
|                |                                                     |
|                | 99% of requests \< 550 ms                           |
+----------------+-----------------------------------------------------+
| DealerSearchV5 | 90 % of requests \< 200 ms                          |
|                |                                                     |
|                | 99% of requests \< 625 ms                           |
+----------------+-----------------------------------------------------+
| Au             | 90 % of requests \< 450 ms                          |
| toSelectDealer |                                                     |
|                | 99% of requests \< 875 ms                           |
+----------------+-----------------------------------------------------+
| LeadTimeSearch | 90 % of requests \< 50 ms                           |
|                |                                                     |
|                | 99% of requests \< 75 ms                            |
+----------------+-----------------------------------------------------+

# **SLI/SLO: Throughput**

**Objective**: Throughput (Traffic) measures how our systems are being
used. This doesn't necessarily impact our service's reliability.
Instead, traffic is potentially a cause of other issues that impact
reliability, like saturation and latency.

**SLI**: The throughput SLI specifies the number of requests that the
service must be able to handle per unit of time, such as requests per
second (TPS) or requests per hour (TPH).

**SLI Calculation:** *count all \"api\" HTTP requests that occur within
a 1-second time window, regardless of their HTTP status code.*

**SLO**: The objective of the throughput Service Level Objective (SLO)
is to ensure that the system is capable of handling a certain volume of
requests within a specified period. The idea is to ensure that the
system can handle the expected workload without experiencing performance
degradation (latency) or failure (availability) as per established
thresholds.
<table>
  <tr>
    <td>DealerSearchV1</td>
    <td>TPS</td>
  </tr>
  <tr>
    <td>DealerSearchV4</td>
    <td>15 TPS</td>
  </tr>
  <tr>
    <td>DealerSearchV5</td>
    <td>15 TPS</td>
  </tr>
  <tr>
    <td>AutoSelectDealer</td>
    <td>15 TPS</td>
  </tr>
  <tr>
    <td>LeadTimeSearch</td>
    <td>15 TPS</td>
  </tr>
</table>

# **Rationale**

Availability and latency SLIs were measured over a period of 28 days.
The SLI/SLO mentioned in this artifact were defined by the product team
and the services were verified to be running at or above those levels.
The 28-day measurement period is used to ensure that the SLIs are
representative of the service\'s overall performance. The services are
verified to be running at or above the defined SLIs by using a variety
of monitoring tools and techniques.

# **Error Budget**

Each objective has a separate error budget, which is defined as the
difference between 100% and the goal for that objective. For example, if
the goal for API availability is 99.5%, then the API availability error
budget is 0.5%.

The error budget is used to determine how many errors are allowed before
the service is considered to be in violation of its SLO. For example, if
the API availability error budget is 5 errors per 1000 requests, then
the service is considered to be in violation of its SLO if there are
more than 5 errors in a given time period.

The error budget will be used to help teams prioritize their work and
make decisions about how to allocate resources. For example, if a team
has a limited budget for fixing errors, they can use the error budget to
determine which errors are the most important to fix.

# **Clarifications and Caveats**

-   The latency metrics for the dealer search service were defined by
    the product team based on the criticality and dependencies of the
    API. The product team determined which latency metrics were most
    important to measure and how they would be used to track the
    performance of the API.

-   The product team also defined the error codes that would be counted
    as errors. HTTP 5XX status messages are considered to be errors
    because they indicate that the API was not able to process the
    request successfully. All other status codes are considered to be
    successes.

-   The availability and latency metrics for the dealer search service
    were determined after considering the SLA for the various Google
    managed cloud services. Where necessary, the SLO metrics for the
    appropriate upstream dependent systems were also taken into account
    before arriving at the target SLO for dealer search service.

**SLO Exclusions**

The SLO does not apply to certain features and circumstances, including:

1.  Features that have been designated as pre-general availability
    (Alpha, Beta), as they are still in development and may not yet meet
    the standards of the SLO.

2.  Errors that are caused by our internal or external integrations, as
    well as upstream systems. These errors are beyond our direct control
    and may have a ripple effect on the performance of the system,
    making it difficult to guarantee the terms of the SLO.

3.  External factors that are outside of our control, such as issues
    with Google's compute layer.

These factors will impact the performance of the system and fall outside
of the scope of the SLO.
