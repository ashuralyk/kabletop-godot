# Introduction

By integrating the Godot game engine's Rust binding library gdnative and `kabletop-ckb-sdk`, the interfaces that interact with the Kabletop contract will be encapsulated into interfaces that the Godot engine can recognize, so that game developers with a small amount of blockchain development experience can connect their games to the CKB network.

Godot game engine supports Windows, Linux and MacOS systems, accordingly, `kabletop-godot` needs to be compiled into a `.dll`, `.so` or `.dylib` file and imported into the Godot engine. It is recommended to refer to the official Godot documentation for the how-to details.


# Godot Interfaces

`kabletop-godot` provides the following types of interfaces for the Godot game engine:

A. Interfaces related to Lua files
> 1. set_entry
> 2. run
> 3. replay

B. Interfaces related to state caching
> 1. set_winner
> 2. get_ckb
> 3. get_cache
> 4. get_box_status
> 5. get_uncomplete_kabletop_caches

C. Interfaces related to NFT
> 1. set_selected_nfts
> 2. get_selected_nfts
> 3. get_selected_nfts_count
> 4. delete_nfts
> 5. transfer_nfts
> 6. issue_nfts
> 7. purchase_nfts
> 8. reveal_nfts
> 9. create_nft_wallet
> 10. get_owned_nfts

D. Interfaces related to P2P
> 1. connect_to
> 2. listen_at
> 3. shutdown
> 5. reply_p2p_message
> 6. send_p2p_message

E. Interfaces related to channel creation and interaction
> 1. sync
> 2. close_game
> 3. create_channel
> 4. close_channel
> 5. challenge_channel

F. Interfaces related to relay server
> 1. register_relay
> 2. unregister_relay
> 3. connect_client_via_relay
> 4. disconnect_client_via_replay
> 5. fetch_clients_from_relay

Note: Refer to the comments in [lib.rs](https://github.com/ashuralyk/kabletop-godot/blob/master/src/lib.rs) for the specific usage of the interfaces. In addition, the Kabletop project is still in the development stage and may modify most of the interfaces in subsequent releases.

# Gameplay writen in Lua

The [kabletop-godot-sdk](https://github.com/ashuralyk/kabletop-godot/tree/master/kabletop-godot-sdk) submodule contains a complete Lua virtual machine for executing Lua code. For example, all processes in a two-player game that have an impact on the outcome of the match need to be written in Lua code, because the Kabletop contract needs to refer to these processes to complete the verification of the on-chain game logic, which should be identical to the local verification logic.

The Kabletop game also needs to run Lua code to complete the gameplay process, so it must have intermediate states during execution. `kabletop-godot` does this by listening to a default global variable, exhausting all the events in it after the code execution, and passing them back to the Godot engine through the event callback interface.

For example，the demo's gameplay logic is exactly writen in [Lua](https://github.com/ashuralyk/kabletop-demo/tree/master/lua).

# P2P Network

`kabletop-godot` has a built-in P2P network module to prevent developers from building their own network module separately. The reason is the process of creating, interacting and closing Kabletop state-channel is complex and requires a lot of CKB development knowledge which is also strongly bound to the network interaction.

Currently, P2P module can only support intranet connection and connection with relay server.