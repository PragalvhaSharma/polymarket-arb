---
title: "获取交易所统计数据"
url: "https://docs.polymarket.com/cn/api-reference/endpoints/perps/get-info-exchange-stats"
---

> ## Documentation Index
> Fetch the complete documentation index at: https://docs.polymarket.com/llms.txt
> Use this file to discover all available pages before exploring further.

# 获取交易所统计数据

> Get aggregate statistics for all pUSD-quoted Polymarket perpetual markets.
The time window is half-open: trades at `start_timestamp` are included and trades at
`end_timestamp` are excluded. The maximum window is 31 days.
Open interest is the one-sided USD notional from the latest complete sample before
`end_timestamp`; its value and timestamp are null when no complete sample is available.
Fees are gross positive maker and taker trading fees. Rebates, incentives, and referral
payments are excluded.




## OpenAPI

````yaml api-spec/perps-openapi.json GET /v1/info/exchange-stats
openapi: 3.0.3
info:
  title: Polymarket Perps HTTP API
  version: 1.0.0
  description: HTTP API for Polymarket perpetual trading system.
  license:
    name: Apache 2.0
    url: https://www.apache.org/licenses/LICENSE-2.0.html
servers:
  - url: https://api.perpetuals.polymarket.com
    description: Production Perps HTTP API
security: []
paths:
  /v1/info/exchange-stats:
    get:
      summary: Get Exchange Statistics
      description: >
        Get aggregate statistics for all pUSD-quoted Polymarket perpetual
        markets.

        The time window is half-open: trades at `start_timestamp` are included
        and trades at

        `end_timestamp` are excluded. The maximum window is 31 days.

        Open interest is the one-sided USD notional from the latest complete
        sample before

        `end_timestamp`; its value and timestamp are null when no complete
        sample is available.

        Fees are gross positive maker and taker trading fees. Rebates,
        incentives, and referral

        payments are excluded.
      operationId: getExchangeStatistics
      parameters:
        - name: start_timestamp
          in: query
          required: true
          schema:
            $ref: '#/components/schemas/start_timestamp'
        - name: end_timestamp
          in: query
          required: true
          schema:
            $ref: '#/components/schemas/end_timestamp'
      responses:
        '200':
          description: Exchange statistics response.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ExchangeStatistics'
        '400':
          $ref: '#/components/responses/Error400Response'
        '408':
          $ref: '#/components/responses/Error408Response'
        '413':
          $ref: '#/components/responses/Error413Response'
        '429':
          $ref: '#/components/responses/Error429Response'
        '500':
          $ref: '#/components/responses/Error500Response'
        '503':
          $ref: '#/components/responses/Error503Response'
      security: []
components:
  schemas:
    start_timestamp:
      type: integer
      description: Start timestamp in milliseconds
      example: 1767225600000
    end_timestamp:
      type: integer
      description: End timestamp in milliseconds
      example: 1767229200000
    ExchangeStatistics:
      type: object
      required:
        - start_timestamp
        - end_timestamp
        - volume
        - open_interest
        - open_interest_timestamp
        - fees
      properties:
        start_timestamp:
          $ref: '#/components/schemas/start_timestamp'
        end_timestamp:
          $ref: '#/components/schemas/end_timestamp'
        volume:
          $ref: '#/components/schemas/exchange_volume'
        open_interest:
          $ref: '#/components/schemas/exchange_open_interest'
        open_interest_timestamp:
          $ref: '#/components/schemas/open_interest_timestamp'
        fees:
          $ref: '#/components/schemas/exchange_fees'
    exchange_volume:
      type: string
      description: >-
        Matched trading volume in USD notional for the requested window, as an
        unformatted numeric string
      example: '10164976.25'
    exchange_open_interest:
      type: string
      nullable: true
      description: >-
        One-sided open interest in USD notional at the latest complete sample
        before the requested window end, as an unformatted numeric string. Null
        when no complete sample is available.
      example: '9238345.12'
    open_interest_timestamp:
      type: integer
      nullable: true
      description: >-
        Timestamp of the open-interest sample in Unix milliseconds. Null when no
        complete sample is available.
      example: 1767225570000
    exchange_fees:
      type: string
      description: >-
        Gross positive maker and taker trading fees in USD for the requested
        window, as an unformatted numeric string. Rebates, incentives, and
        referral payments are excluded.
      example: '1245.67'
    Error400:
      title: Error400
      type: object
      required:
        - status
        - error
      properties:
        status:
          type: string
          enum:
            - err
        error:
          $ref: '#/components/schemas/error'
    Error408:
      title: Error408
      type: object
      required:
        - status
        - error
      properties:
        status:
          type: string
          enum:
            - err
        error:
          $ref: '#/components/schemas/error'
    Error413:
      title: Error413
      type: object
      required:
        - status
        - error
      properties:
        status:
          type: string
          enum:
            - err
        error:
          $ref: '#/components/schemas/error'
    Error429:
      title: Error429
      type: object
      required:
        - status
        - error
      properties:
        status:
          type: string
          enum:
            - err
        error:
          $ref: '#/components/schemas/error'
    Error500:
      title: Error500
      type: object
      required:
        - status
        - error
      properties:
        status:
          type: string
          enum:
            - err
        error:
          $ref: '#/components/schemas/error'
    Error503:
      title: Error503
      type: object
      required:
        - status
        - error
      properties:
        status:
          type: string
          enum:
            - err
        error:
          $ref: '#/components/schemas/error'
    error:
      type: string
      description: >-
        Error identifier. For domain rejections and transport errors
        (`401`/`404`/`429`/`500`) this is a stable, machine-readable snake_case
        identifier that is part of the API contract and safe to branch on, e.g.
        `insufficient_margin`, `insufficient_balance`, `order_not_found`,
        `reduce_only_invalid`, `price_outside_bounds`, `position_not_found`,
        `invalid_margin_mode`, `invalid_margin_amount`,
        `margin_below_required_initial`, `account_liquidating`, `unauthorized`,
        `not_found`. For `400` it is a human-readable validation detail whose
        wording may change. See the Error handling guide for the domain
        identifiers. (Post-only / Fill-or-Kill outcomes are order statuses such
        as `post_only_rejected`, not rejections.)
      example: insufficient_margin
  responses:
    Error400Response:
      description: |
        Bad request — the request was malformed or failed validation (bad query
        parameters, unparseable body, invalid signature, or a domain pre-check).
        The `error` field is a human-readable validation detail.
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error400'
    Error408Response:
      description: |
        Request Timeout — the request body was not delivered within the
        gateway's read deadline. Protects against slow uploads holding
        connections open; send the complete body promptly and retry.
        `error` is `request_body_timeout`.
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error408'
    Error413Response:
      description: |
        Content Too Large — the request body exceeds the gateway's size cap,
        judged on the declared `Content-Length` or the actual stream. Split
        oversized batches into smaller requests. `error` is
        `payload_too_large`.
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error413'
    Error429Response:
      description: >
        Too Many Requests. `error` distinguishes the limit that was hit:

        `ip_rate_limited` (per-IP token bucket), `action_rate_limited`
        (per-account

        action rate), or `open_orders_limit` (resting open-order cap).
      headers:
        Retry-After:
          description: >
            Whole seconds to wait before retrying. Present only on token-bucket

            rate-limit rejections (`ip_rate_limited` and `action_rate_limited`);
            a

            conservative estimate of when enough capacity will have refilled to

            admit the request. Absent on `open_orders_limit`, which is a
            capacity

            limit, not a rate limit — waiting does not free order slots; cancel

            resting orders or wait for fills instead.
          schema:
            type: integer
            example: 2
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error429'
    Error500Response:
      description: |
        Internal server error. `error` is `internal_error`.
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error500'
    Error503Response:
      description: |
        Service unavailable — the request was deliberately shed and did not
        execute; it is safe to retry with backoff, honoring `Retry-After` when
        present. Causes include the replica's pod-level in-flight cap (shed
        before dispatch, with `Retry-After`), a database read shed as
        backpressure under overload (by the local read gate or by the database
        server's concurrency cap — the query never executed), a required
        internal cache that is stale or not yet primed and would return wrong
        data, or a full bounded deferred-write queue. `error` is
        `service_unavailable`.
      headers:
        Retry-After:
          description: |
            Whole seconds to wait before retrying. Present on pre-dispatch
            in-flight-cap sheds — a transient overload signal, so the hint is
            short and constant.
          schema:
            type: integer
            example: 1
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error503'

````