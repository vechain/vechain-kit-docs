# WalletButton

VeChain Kit provides multiple ways to customize the UI components. Here are some examples of different button styles and variants.

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

Usage example

```typescript
'use client';

import { VStack, Text, Button, Box, HStack, Grid } from '@chakra-ui/react';
import {
    WalletButton,
    useAccountModal,
    ProfileCard,
    useWallet,
} from '@vechain/vechain-kit';
import { MdBrush } from 'react-icons/md';
import { CollapsibleCard } from '../../ui/CollapsibleCard';

export function UIControls() {
    const { open } = useAccountModal();
    const { account } = useWallet();

    return (
        <>
            <VStack spacing={6} align="stretch" w={'full'}>
                <Text textAlign="center">
                    VeChain Kit provides multiple ways to customize the UI
                    components. Here are some examples of different button
                    styles and variants.
                </Text>

                <HStack w={'full'} justifyContent={'space-between'}>
                    {/* Mobile Variants */}
                    <HStack w={'full'} justifyContent={'center'}>
                        <VStack
                            w={'fit-content'}
                            spacing={6}
                            p={6}
                            borderRadius="md"
                            bg="whiteAlpha.50"
                        >
                            <Text fontWeight="bold">
                                Account Button Variants
                            </Text>
                            <Text
                                fontSize="sm"
                                textAlign="center"
                                color="gray.400"
                            >
                                Note: Some variants might look different based
                                on connection state and available data. Eg:
                                "iconDomainAndAssets" will show the assets only
                                if the user has assets. And same for domain
                                name.
                            </Text>
                            <Grid
                                templateColumns={{
                                    base: '1fr',
                                    md: 'repeat(2, 1fr)',
                                }}
                                gap={8}
                                w="full"
                                justifyContent="space-between"
                            >
                                {/* First Column Items */}
                                <VStack alignItems="flex-start" spacing={8}>
                                    <VStack alignItems="flex-start" spacing={2}>
                                        <Box w={'fit-content'}>
                                            <WalletButton
                                                mobileVariant="icon"
                                                desktopVariant="icon"
                                            />
                                        </Box>
                                        <Text
                                            fontSize="sm"
                                            fontWeight="medium"
                                            color="blue.300"
                                            bg="whiteAlpha.100"
                                            px={3}
                                            py={1}
                                            borderRadius="full"
                                        >
                                            variant: "icon"
                                        </Text>
                                    </VStack>

                                    <VStack alignItems="flex-start" spacing={2}>
                                        <Box w={'fit-content'}>
                                            <WalletButton
                                                mobileVariant="iconAndDomain"
                                                desktopVariant="iconAndDomain"
                                            />
                                        </Box>
                                        <Text
                                            fontSize="sm"
                                            fontWeight="medium"
                                            color="blue.300"
                                            bg="whiteAlpha.100"
                                            px={3}
                                            py={1}
                                            borderRadius="full"
                                        >
                                            variant: "iconAndDomain"
                                        </Text>
                                    </VStack>

                                    <VStack alignItems="flex-start" spacing={2}>
                                        <Box w={'fit-content'}>
                                            <WalletButton
                                                mobileVariant="iconDomainAndAddress"
                                                desktopVariant="iconDomainAndAddress"
                                            />
                                        </Box>
                                        <Text
                                            fontSize="sm"
                                            fontWeight="medium"
                                            color="blue.300"
                                            bg="whiteAlpha.100"
                                            px={3}
                                            py={1}
                                            borderRadius="full"
                                        >
                                            variant: "iconDomainAndAddress"
                                        </Text>
                                    </VStack>
                                </VStack>

                                {/* Second Column Items */}
                                <VStack alignItems={'flex-start'} spacing={8}>
                                    <VStack alignItems="flex-start" spacing={2}>
                                        <Box w={'fit-content'}>
                                            <WalletButton
                                                mobileVariant="iconDomainAndAssets"
                                                desktopVariant="iconDomainAndAssets"
                                            />
                                        </Box>
                                        <Text
                                            fontSize="sm"
                                            fontWeight="medium"
                                            color="blue.300"
                                            bg="whiteAlpha.100"
                                            px={3}
                                            py={1}
                                            borderRadius="full"
                                        >
                                            variant: "iconDomainAndAssets"
                                        </Text>
                                    </VStack>

                                    <VStack alignItems="flex-start" spacing={2}>
                                        <Box w={'fit-content'}>
                                            <WalletButton
                                                mobileVariant="iconDomainAndAssets"
                                                desktopVariant="iconDomainAndAssets"
                                                buttonStyle={{
                                                    border: '2px solid #000000',
                                                    boxShadow:
                                                        '-2px 2px 3px 1px #00000038',
                                                    background: '#f08098',
                                                    color: 'white',
                                                    _hover: {
                                                        background: '#db607a',
                                                        border: '1px solid #000000',
                                                        boxShadow:
                                                            '-3px 2px 3px 1px #00000038',
                                                    },
                                                    transition: 'all 0.2s ease',
                                                }}
                                            />
                                        </Box>
                                        <Text
                                            fontSize="sm"
                                            fontWeight="medium"
                                            color="blue.300"
                                            bg="whiteAlpha.100"
                                            px={3}
                                            py={1}
                                            borderRadius="full"
                                        >
                                            variant: "iconDomainAndAssets"
                                            (styled)
                                        </Text>
                                    </VStack>

                                    <VStack alignItems="flex-start" spacing={2}>
                                        <Button onClick={open}>
                                            <Text>This is a custom button</Text>
                                        </Button>
                                        <Text
                                            fontSize="sm"
                                            fontWeight="medium"
                                            color="blue.300"
                                            bg="whiteAlpha.100"
                                            px={3}
                                            py={1}
                                            borderRadius="full"
                                        >
                                            no variant, custom button
                                        </Text>
                                    </VStack>
                                </VStack>
                            </Grid>
                        </VStack>
                    </HStack>
                </HStack>
            </VStack>
        </>
    );
}

```
