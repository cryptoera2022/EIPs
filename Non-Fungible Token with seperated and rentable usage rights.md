---
eip: <to be assigned>
title: Non-Fungible Token with seperated and rentable usage rights	//带有独立可租赁使用权的NFT
description: <Description is one full (short) sentence>
author: tech@derivstudio.com
discussions-to: 
status: Draft
type: Standards Track
created: 2022-04-17
requires (*optional): 1155
---

## Abstract
This standard is an extension of ERC-1155. It proposes introducing the concept of independent, multiple, and leasable rights of use to enable NFT to be leased out for different cycles while ownership remains with the owner.

## Motivation
The traditional ERC721 and ERC1155 focus more on ownership. However, NFTs as digital assets are more prominent in use than ownership. Taking artistic NFTs as an example, NFT artists may wish to rent out the use rights of their works to media companies in the allotted time, or NFT musicians may wish to make their music available to listeners as per playing duration. 
Therefore, to better serve NFT developers to meet such needs and develop more sophisticated NFT products, we propose directly introducing rentable usage rights to complement the ERC standard.


## Specification
The key words “MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”, “SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “MAY”, and “OPTIONAL” in this document are to be interpreted as described in RFC 2119.

```
pragma solidity ^0.8.0;

interface IRental {
    // Logged when the user of a NFT is changed or expires is changed
    /// @notice Emitted when the `user` of an NFT or the `expires` of the `user` is changed
    /// The zero address for user indicates that there is no user address
    event Rented(
        uint256 indexed id,
        address indexed user,
        uint256 amount,
        uint256 expires
    );

    event TakeBack(uint256 indexed id, address indexed user, uint256 amount);

    /// @notice Function to rent out usage rights
    /// @param from the address to approve
    /// @param to the address to rent the NFT usage rights
    /// @param id the NFT ID
    /// @param amount number of usage right amount
    /// @param expires the appointed rent period
    function safeRent(
        address from,
        address to,
        uint256 id,
        uint256 amount,
        uint256 expires
    ) external;

    /// @notice Function to take back usage rights
    /// @param user the address who rent the NFT's usage rights
    /// @param tokenId the NFT ID
    /// @param amount number of rented usage right amount
    function takeBack(
        address user,
        uint256 tokenId,
        uint256 amount
    ) external;

    function ownerOf(uint256 id) external view returns (address);

    function totalSupply(uint256 id) external view returns (uint256);
}

```
## Rationale
There are two main benefits to creating this proposal. One is to make it possible to create NFTs with detachable usage rights, of which the transfer of usage rights is separated from the transfer of ownership. The NFT owner can execute the #safeRent function to lease out the usage rights to other users and the #takeBack function to retrieve the usage rights after the lease expiration.

## Backwards Compatibility
As mentioned at the beginning, this is an extension of ERC1155. Therefore, it is fully compatible with ERC1155.

## Security Considerations
There are no security considerations directly related to the implementation of this standard.

## Copyright
Disclaimer of copyright and related rights through [CC0](../LICENSE.md).
