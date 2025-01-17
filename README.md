# CCash

A webserver hosting a bank system for Minecraft, able to be used from web browser or from CC/OC if you're playing modded.

The currency model usually employed is diamond-based, however, this is fraught with issues:

- The primary issue is minecraft worlds are infinite leading to hyper inflation as everyone accrues more diamonds
- There is no central authority minting the currency, any consumer can introduce more diamonds to the system
- Some resources are passively reapable, making the generation of currency a larger focus then of products
- Locality is required for transaction
- Theft is possible, ownership is possession based

CCash solves these issues and adds a level of abstraction, the main philosophy of CCash is to have fast core operations that other services build on

## Build

Drogon depedencies (varies by OS/distro)
```
# Debian
sudo apt install libjsoncpp-dev uuid-dev openssl libssl-dev zlib1g-dev

# macOS
brew install jsoncpp ossp-uuid openssl zlib
```

Building the project

```
git clone --recurse-submodule https://github.com/EntireTwix/CCash/
cd CCash
mkdir build
cd build
cmake ..
make -j<threads>
```

Then edit config.json to include the paths to your certs for HTTPS (I use certbot), or just remove the listener for port 443.

```
vim ../config.json
```

Finally, run the program

```
sudo ./bank <admin password> <saving frequency in minutes> <threads>
```

## Connected Services

Using the Bank's API allows (you/others) to (make/use) connected services that utilize the bank, a couple ideas can be found [here](services.md)

## Developing for this project

Find [Language APIs](https://github.com/EntireTwix/CCash/blob/main/APIs.md) here

Find [endpoints](https://github.com/EntireTwix/CCash/blob/main/help.md) here

## FAQ

> "How is money initially injected into the economy?"

You can choose whichever approach you would like, but I would personally recommend a one-way exchange via the CC ATM for a minable currency. A good resource for this is **coal**, as it is not farmable - but also not so useful/rare that there is nothing you want more (diamonds). This rewards early adopters, and creates a sunk cost effect (the resource is promptly burnt).

## [Contributions](https://github.com/EntireTwix/CCash/graphs/contributors)
Thank you to the contributors

| Name                                        | Project Work                                                            | Connected Service Work |
| :------------------------------------------ | ----------------------------------------------------------------------- | ---------------------- |
| [Expand](https://github.com/Expand-sys)     | Slight docker changes                                                   | Frontend               |
| [React](https://github.com/Reactified)      | CC API, Logo                                                            | CC Shop, CC ATM.       |
| [Doggo](https://github.com/FearlessDoggo21) | Logs loading/adding Optimized, HTTP convention suggestions, Python API  | `N/A`                  |
| [Luke](https://github.com/LukeeeeBennett)   | JS API, Docker, Slight Doc edits                                        | `N/A`                  |
| [Jolly](https://github.com/STBoyden)        | Slight Doc edits                                                        | `N/A`                  |

## Features

### Performance
- In memory database instead of on disk
- **NOT** written in Lua, like a OC/CC implementation
- Written in **C++**, arguably the fastest language
- **Multi-threaded**
- **Parallel hashmaps** a far [superior](https://greg7mdp.github.io/parallel-hashmap/) HashMap implementation to the STD, that also benefits from multi-threaded
- **Drogon** is a very fast [web framework](https://www.techempower.com/benchmarks/#section=data-r20&hw=ph&test=composite)
- **xxHash** for the hashing of passwords, it is very fast: [graph](https://user-images.githubusercontent.com/750081/61976089-aedeab00-af9f-11e9-9239-e5375d6c080f.png)
- **Lightweight**, anecodotally I experienced (on my laptop's i7 6700K, 8 threads):
  - Memory usage of 8.5 MB (with 0 users)
  - 0.0% CPU usage idle 
  - <1% CPU on average 
  - 1000 requests in parallel completed in 0.85s which spiked CPU usage to 7%
  
### Safety

- **Tamper Proof** relative to an in-game implementation
- **Auto-Saving** and Saves on close
- All passwords are **Hashed**
- **HTTPS** (OpenSSL)

### Accessibility

- **RESTful** API for connected services like a market, gambling, or anything else you can think of
- Able to be used millions of blocks away, across dimensions, servers, **vanilla or modded**.
- **Logging** of all transactions, configurable in [consts.hpp](include/consts.hpp)

### Other
- **Return balance on deletion**, configurable in [consts.hpp](include/consts.hpp)

## Dependencies

- [Parallel HashMap](https://github.com/greg7mdp/parallel-hashmap/tree/master)
- [Drogon](https://github.com/an-tao/drogon/tree/master)
- [XXHASH](https://github.com/Cyan4973/xxHash)
