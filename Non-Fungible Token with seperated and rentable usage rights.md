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
this standard is an extention of ERC-1155. It proposes seperated, multiple usage rights which can be rented for designated period while the proprietorship is still belongs to owner.

本标准是ERC1155的扩展. 它引入了独立的/多份的/可租赁的使用权, 该使用权可以按照不同周期租赁出去,同时NFT的使用权仍然属于owner.

## Motivation
NFT tokens like ERC721 and ERC1155 emphasize more on ownership. However, NFT tokens have more usage scenarios than holding. Picture owners can rent out the picture to different clients for different periods. Music owners can sell to multiple users according to playback duration. Thus, it would be useful for developers to deploy more complex NFT products.


传统的ERC721和ERC1155更多关注所有权, 然而, 作为数字资产, NFT的使用特性比拥有特性更常用. 例如, 图片的拥有者可以按时间周期向工厂出租其使用权, 或者音乐的所有者可以按播放时间允许用户收听. 所以我们通过直接引入可租赁的使用权,更方便开发者使用NFT.


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
#Seperated usage rights
If NFT owner does not intend to transfer or mortgage the ownership when its value is realized, the safeRent function enables owner to rent NFT for appointed period, and takeBack function enables the usage rights return to owner.


//创立本提案的两个主要好处, 一是实现了分离的使用权, 这样就可以在不转移所有权的情况下方便的授权.
//safeRent函数帮助所有者将使用权出租给其他用户, 到期后执行takeBack函数可取回使用权.


## Backwards Compatibility
As mentioned at the very beginning, it is an extension interface of ERC1155, hence it is 100% compatible with ERC-1155.

如开头所说, 这是ERC1155的扩展, 所以, 完全兼容ERC1155.

## Security Considerations
There are no security considerations related directly to the implementation of this standard.


## Copyright
Copyright and related rights waived via [CC0](../LICENSE.md).
