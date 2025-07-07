# Upgrade VeChain Kit from 1.x to 2.x

In 2.0 we replaced connex with sdk.

`useConnex()` is not exported anymore, in favour of useThor() and the signer object can be used to sign transactions following the SDK patterns.

It might require to `rm -rf node_modules yarn.lock && yarn`

`useCallClause` hook now provides type safe contract call clause with return types.

The following hooks were also removed in order to improve permeances.

<details>

<summary>Deprecated Hooks</summary>

Utils Hooks

* useRoundAppVotes
* useSustainabilityActions

Galaxy Member Hooks

* useGMbalance
* useB3trToUpgrade
* useB3trToUpgradeToLevel
* useGetNodeIdAttached
* useGetTokenIdAttachedToNode
* useGMMaxLevel
* useParticipatedInGovernance
* useTokenIdByAccount
* useNFTImage
* useB3trDonated
* useGMBaseUri
* useSelectedTokenId
* useIsGMClaimable
* useSelectedGmNft
* useLevelOfToken
* useNFTMetadataUri

NodeManagement

* useGetNodeManager
* useIsNodeHolder
* useUserXNodes

VeBetterPassport

* useAccountLinking
* usePassportChecks
* useUserDelegation
* useUserStatus
* useAppSecurityLevel
* useGetCumulativeScoreWithDecay
* useGetDelegatee
* useGetDelegator
* useGetEntitiesLinkedToPassport
* useGetPassportForEntity
* useGetPendingDelegationsDelegateePOV
* useGetPendingDelegationsDelegatorPOV
* useGetPendingLinkings
* useIsEntity
* useIsPassportCheckEnabled
* useIsPassport
* useParticipationScoreThreshold
* useSecurityMultiplier
* useThresholdParticipationScore
* useThresholdParticipationScoreAtTimepoint
* useIsBlacklisted
* useIsWhitelisted
* useUserRoundScore

VBD VoterRewards:

* useLevelMultiplier

X2Earn Apps:

* useUserVotesInAllRounds
* useUserTopVotedApps

- useXNode
- useAppAdmin
- useAppExists
- useAppsEligibleInNextRound
- useGetX2EarnAppAvailableFunds
- useXAppsMetadataBaseUri
- useXNodeCheckCooldown

XAllocation Voting

* useAllocationAmount

- useXAppVotesQf

</details>







Where you using any of those hooks? Please reach us on Github to add them back
