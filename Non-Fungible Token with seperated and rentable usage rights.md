---
eip: <to be assigned>
title: Extending ERC1155 with rentable usage rights
description: This standard adds multiple usage rights to ERC1155 and makes it rentable separately
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
    /**
     * @notice This emits when user rent NFT
     * @param id The id of the current token
     * @param user The address to rent the NFT usage rights
     * @param amount The amount of usage rights
     * @param expires The specified period of time to rent
     **/
    event Rented(
        uint256 indexed id,
        address indexed user,
        uint256 amount,
        uint256 expires
    );

    /**
     * @notice This emits when the NFT owner takes back the usage rights from the tenant (the `user`) 
     * @param id The id of the current token
     * @param user The address to rent the NFT's usage rights
     * @param amount Amount of usage rights
     **/
    event TakeBack(uint256 indexed id, address indexed user, uint256 amount);

    /**
     * @notice Function to rent out usage rights
     * @param from The address to approve
     * @param to The address to rent the NFT usage rights
     * @param id The id of the current token
     * @param amount The amount of usage rights
     * @param expires The specified period of time to rent
     **/
    function safeRent(
        address from,
        address to,
        uint256 id,
        uint256 amount,
        uint256 expires
    ) external;

    /**
     * @notice Function to take back usage rights after the end of the tenancy
     * @param user The address to rent the NFT's usage rights
     * @param tokenId The id of the current token
     * @param amount The amount of usage rights
     **/
    function takeBack(
        address user,
        uint256 tokenId,
        uint256 amount
    ) external;

    /**
    * @notice Return the NFT to the NFT owner’s address
    **/
    function ownerOf(uint256 id) external view returns (address);

    /**
    * @notice Return the total supply amount of the current token
    **/
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