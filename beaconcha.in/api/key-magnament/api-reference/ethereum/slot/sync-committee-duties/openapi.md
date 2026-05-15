> ## Documentation Index
> Fetch the complete documentation index at: https://docs.beaconcha.in/llms.txt
> Use this file to discover all available pages before exploring further.

# Rewards

> Provides status and reward information about sync committee duties for a specific slot for each validator. 
The validator filter is optional; if omitted, results include all validators assigned sync duties for the slot.
This endpoint currently only supports finalized data.

Use the reward endpoints for epoch based attestation rewards information.

You can combine this endpoint with:
- Find periods your validators were part in:
  [v2/ethereum/validators/sync-committee-periods](/api-reference/ethereum/validators/sync-committee-periods)
- Sync committee overview:
  [v2/ethereum/sync-committee](/api-reference/ethereum/sync-committee/overview)




## OpenAPI

````yaml /v3/openapi3.yaml post /api/v2/ethereum/slot/sync-committee-duties
openapi: 3.0.4
info:
  title: External Service API
  version: 1.0.3
servers:
  - url: https://beaconcha.in
    description: Production API
security:
  - ApiKeyAuth: []
paths:
  /api/v2/ethereum/slot/sync-committee-duties:
    post:
      tags:
        - Slot
      summary: Rewards
      description: >
        Provides status and reward information about sync committee duties for a
        specific slot for each validator. 

        The validator filter is optional; if omitted, results include all
        validators assigned sync duties for the slot.

        This endpoint currently only supports finalized data.


        Use the reward endpoints for epoch based attestation rewards
        information.


        You can combine this endpoint with:

        - Find periods your validators were part in:
          [v2/ethereum/validators/sync-committee-periods](/api-reference/ethereum/validators/sync-committee-periods)
        - Sync committee overview:
          [v2/ethereum/sync-committee](/api-reference/ethereum/sync-committee/overview)
      operationId: GetSlotSyncCommitteeDuties
      requestBody:
        $ref: '#/components/requestBodies/slotOptionalValidatorChainCursorPageSize'
      responses:
        '200':
          $ref: '#/components/responses/SlotSyncCommittee'
        '400':
          $ref: '#/components/responses/BadRequest'
        '401':
          $ref: '#/components/responses/Unauthorized'
        '403':
          $ref: '#/components/responses/Forbidden'
        '404':
          $ref: '#/components/responses/NotFound'
        '405':
          $ref: '#/components/responses/MethodNotAllowed'
        '429':
          $ref: '#/components/responses/RateLimitExceeded'
        '500':
          $ref: '#/components/responses/InternalServerError'
        default:
          $ref: '#/components/responses/DefaultError'
components:
  requestBodies:
    slotOptionalValidatorChainCursorPageSize:
      description: '`validator` is optional in this request body'
      content:
        application/json:
          schema:
            allOf:
              - type: object
                description: >
                  Specify a slot using one of the slot number. Selecting by a
                  consensus view may soon be available in a future update.
                properties:
                  slot:
                    $ref: '#/components/schemas/SlotByNumber'
                required:
                  - slot
              - type: object
                properties:
                  chain:
                    $ref: '#/components/schemas/Chain'
                  cursor:
                    $ref: '#/components/schemas/Cursor'
                  page_size:
                    $ref: '#/components/schemas/PageSize'
                  validator:
                    $ref: '#/components/schemas/nullableValidatorSelector'
            required:
              - slot
              - chain
          example:
            slot:
              number: 2375680
            chain: mainnet
  responses:
    SlotSyncCommittee:
      description: Successful response.
      content:
        application/json:
          schema:
            allOf:
              - $ref: '#/components/schemas/SlotSyncCommittee.ContainerList'
              - $ref: '#/components/schemas/PagingTemplate'
            description: >-
              Response containing detailed information about past or future sync
              committee duties.
    BadRequest:
      description: Bad Request
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
          example:
            error: Bad request. Please check your input and try again.
    Unauthorized:
      description: Unauthorized
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
          example:
            error: Unauthorized. Please provide a valid API key.
    Forbidden:
      description: Forbidden
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
          example:
            error: >-
              endpoint not allowed for your subscription tier. upgrade your
              subscription at https://beaconcha.in/pricing.
    NotFound:
      description: Not Found
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
          example:
            error: The requested resource was not found.
    MethodNotAllowed:
      description: Method Not Allowed
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
          example:
            error: 'method not allowed: GET. all public API endpoints use POST.'
    RateLimitExceeded:
      description: Rate Limit Exceeded
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
          example:
            error: Rate limit exceeded. Please try again later.
    InternalServerError:
      description: Internal Server Error
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
          example:
            error: internal server error. please try again later.
    DefaultError:
      description: An unexpected error response.
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
          example:
            error: An unexpected error occurred.
  schemas:
    SlotByNumber:
      title: Number
      type: object
      properties:
        number:
          $ref: '#/components/schemas/Slot'
      required:
        - number
    Chain:
      type: string
      enum:
        - mainnet
        - hoodi
      default: mainnet
      description: The Ethereum chain to query.
    Cursor:
      type: string
      description: >-
        Cursor value for pagination. See our [pagination guide](/api/pagination)
        for more details.
      default: ''
    PageSize:
      type: integer
      description: The number of items to return per page.
      minimum: 1
      maximum: 10
      default: 10
    nullableValidatorSelector:
      description: >-
        `validator` is optional in this request body. If not provided, results
        will be unfiltered.
      nullable: true
      default: null
      allOf:
        - $ref: '#/components/schemas/validatorsSelector'
    SlotSyncCommittee.ContainerList:
      type: object
      properties:
        data:
          type: array
          items:
            $ref: '#/components/schemas/SlotSyncCommittee.Data'
      required:
        - data
    PagingTemplate:
      type: object
      properties:
        paging:
          $ref: '#/components/schemas/Paging'
    Error:
      type: object
      properties:
        error:
          type: string
    Slot:
      type: integer
      minimum: 0
      description: Slot by number.
    validatorsSelector:
      description: >-
        Free selectors available to all users:

        - validator_identifiers: One or more validator indices or public keys to
        filter by.

        - dashboard_id: Your beaconcha.in dashboard ID (requires a free
        account).
          
        **Premium selectors** for Scale & Enterprise plans
        (https://beaconcha.in/pricing):

        - withdrawal: The validator's withdrawal credential or the Ethereum
        wallet address used for withdrawals.

        - deposit_address: The Ethereum wallet address used for the validator's
        deposit.

        - entity: The name of the assigned entity (e.g., "Lido", "Coinbase").
        Optionally include `sub_entity` for more specific filtering. Matching is
        case-insensitive.


        Note: The set of validators matched by `deposit_address` and
        `withdrawal` selectors is updated once per epoch (~6.4 minutes). Newly
        deposited validators may take up to one epoch to appear in query
        results.


        Note: The set of validators matched by `entity` selector is updated once
        per day.
          
      oneOf:
        - $ref: '#/components/schemas/ValidatorsByIdentifiers'
        - $ref: '#/components/schemas/ValidatorsByDashboard'
        - $ref: '#/components/schemas/ValidatorsByDeposit'
        - $ref: '#/components/schemas/ValidatorsByWithdrawal'
        - $ref: '#/components/schemas/ValidatorsByEntity'
    SlotSyncCommittee.Data:
      type: object
      properties:
        validator:
          $ref: '#/components/schemas/validator'
        reward:
          $ref: '#/components/schemas/rewardBreakdown'
          description: >
            This field returns only finalized rewards. If the rewards are not
            finalized, 

            the value will be null.
          nullable: true
        status:
          $ref: '#/components/schemas/SyncDutyStatus'
        finality:
          $ref: '#/components/schemas/FinalityParams'
      required:
        - validator
        - status
        - finality
    Paging:
      type: object
      properties:
        next_cursor:
          description: >-
            Cursor to the next page of results. See our [pagination
            guide](/api/pagination) for more details.
          type: string
    ValidatorsByIdentifiers:
      type: object
      title: Indices/Pubkeys
      properties:
        validator_identifiers:
          $ref: '#/components/schemas/validatorIndexPublicKeys'
      required:
        - validator_identifiers
    ValidatorsByDashboard:
      type: object
      title: Dashboard
      properties:
        dashboard_id:
          $ref: '#/components/schemas/dashboardID'
        group_id:
          $ref: '#/components/schemas/dashboardGroupID'
      required:
        - dashboard_id
    ValidatorsByDeposit:
      type: object
      title: 💎 Deposit
      properties:
        deposit_address:
          $ref: '#/components/schemas/ExecutionLayerAddress'
      required:
        - deposit_address
    ValidatorsByWithdrawal:
      type: object
      title: 💎 Withdrawal
      properties:
        withdrawal:
          $ref: '#/components/schemas/AddressOrCredential'
      required:
        - withdrawal
    ValidatorsByEntity:
      type: object
      title: 💎 Entity
      description: >
        Select validators by their assigned entity (e.g., staking provider) and
        optionally a sub-entity.

        Entity matching is case-insensitive.
      properties:
        entity:
          type: string
          description: >
            The name of the entity to filter validators by (e.g., "Lido",
            "Coinbase").

            Matching is case-insensitive.
        sub_entity:
          type: string
          description: >
            Optional sub-entity name to further filter validators within the
            entity.

            Matching is case-insensitive.
      required:
        - entity
    validator:
      type: object
      properties:
        index:
          allOf:
            - $ref: '#/components/schemas/validatorIndex'
          nullable: true
        public_key:
          $ref: '#/components/schemas/validatorPublicKey'
    rewardBreakdown:
      allOf:
        - $ref: '#/components/schemas/rewardBreakdownBase'
    SyncDutyStatus:
      type: string
      description: >
        Indicates the status of the sync committee duty assigned to the
        validator for the specified slot.


        Possible values:
          - "scheduled": The duty is scheduled for a future slot or the current head slot. Duties can only be performed starting from the next slot.
          - "success": The validator successfully performed the sync committee duty for this slot.
          - "missed": The validator missed the assigned sync committee duty for this slot.

        For future slots, the status will always be "scheduled". For the current
        head slot, the status is also "scheduled" since duties are only eligible
        for inclusion in the next slot.
      enum:
        - success
        - missed
        - scheduled
    FinalityParams:
      type: string
      description: |
        Indicates the finality status of the data provided. 
          - Finalized data cannot be changed without slashing at least one-third of all validators, providing strong economic guarantees.
          - Data marked as not_finalized does not have this guarantee and may still change.
      enum:
        - not_finalized
        - finalized
    validatorIndexPublicKeys:
      description: >
        An array containing either validator indices or public keys. Index and
        public key can be mixed in the same array.


        Subscribed users (Hobbyist, Business, and Scale tiers) can include up to
        100 entries; free trial users and legacy subscription users (Sapphire,
        Emerald, Diamond) are limited to 20.
      type: array
      items:
        $ref: '#/components/schemas/validatorIndexPublicKey'
      minItems: 1
      maxItems: 100
    dashboardID:
      description: >
        beaconcha.in dashboard ID. You can find your dashboard ID in the URL of
        your dashboard page on beaconcha.in (e.g.,
        https://beaconcha.in/dashboard/12345).
      type: integer
      x-go-type: '*int'
      minimum: 0
    dashboardGroupID:
      description: >-
        Optional beaconcha.in dashboard group ID. If no group ID is provided,
        all validators in the dashboard are considered.
      type: integer
      minimum: 0
      nullable: true
    ExecutionLayerAddress:
      type: string
      pattern: ^0x[a-fA-F0-9]{40}$
      description: A standard Ethereum address (20-byte hex string with 0x prefix).
    AddressOrCredential:
      type: string
      pattern: ^(0x)?[0-9a-fA-F]{40}$|^(0x)?0[012][0-9a-fA-F]{62}$
      description: >
        Either an execution layer address (20-byte hex string with 0x prefix) or
        a full 32-byte withdrawal credential.
    validatorIndex:
      description: Validator Index
      type: integer
      minimum: 0
    validatorPublicKey:
      type: string
      description: Public key of a validator
      pattern: ^0x[a-fA-F0-9]{96}$
    rewardBreakdownBase:
      type: object
      properties:
        total:
          $ref: '#/components/schemas/wei'
          description: >
            Net result for this duty (reward minus penalty).


            Positive values indicate net rewards earned; negative values
            indicate net penalties.
        reward:
          $ref: '#/components/schemas/wei'
          description: >
            Reward is the amount earned for correctly and promptly performing
            duties. Represents the positive balance increase received.
        penalty:
          $ref: '#/components/schemas/wei'
          description: >
            Penalty is the amount deducted from your balance for missing or
            incorrectly performing duties.

            This is always a positive value, representing the loss incurred in
            addition to missed rewards.


            When missing a duty, you earn no reward and also incur a penalty,
            resulting in a net negative impact on your balance.
        missed_reward:
          $ref: '#/components/schemas/wei'
          description: >
            Missed rewards are the potential earnings lost due to missed or
            delayed duties that you could have earned if performed correctly. 

            This value shows the difference between the maximum possible reward
            and the actual reward received.


            Returned as a positive value representing unearned rewards.
      required:
        - total
        - reward
        - penalty
        - missed_reward
    validatorIndexPublicKey:
      oneOf:
        - $ref: '#/components/schemas/validatorIndex'
        - $ref: '#/components/schemas/validatorPublicKey'
    wei:
      type: string
      description: Amount in wei (1 ETH = 10^18 wei)
      pattern: ^(0|-?[1-9][0-9]*)$
  securitySchemes:
    ApiKeyAuth:
      type: http
      scheme: bearer
      description: >-
        Authorization header with value: Bearer YOUR_TOKEN. Refer to the [API
        Keys](/api/overview#api-keys) section to create your API key.

````
