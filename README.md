# Cargo semver demo



## Workspace structure

```
crate-a
  - sourcemap = "5"
  - serde = "1.0.144"

crate-b
  - sourcemap = "6"
  - serde = "1.0.143"
```

Dependencies:
- sourcemap: Semver incompatible(Different major version)
- serde: Semver compatible(Same major version, and its major version is not started with 0)


## Cargo.lock

```toml
[[package]]
name = "crate-a"
version = "0.1.0"
dependencies = [
 "serde",
 "sourcemap 5.0.0",
]

[[package]]
name = "crate-b"
version = "0.1.0"
dependencies = [
 "serde",
 "sourcemap 6.1.0",
]

[[package]]
name = "serde"
version = "1.0.144"
source = "registry+https://github.com/rust-lang/crates.io-index"
checksum = "0f747710de3dcd43b88c9168773254e809d8ddbdf9653b84e2554ab219f17860"
dependencies = [
 "serde_derive",
]

[[package]]
name = "serde_derive"
version = "1.0.144"
source = "registry+https://github.com/rust-lang/crates.io-index"
checksum = "94ed3a816fb1d101812f83e789f888322c34e291f894f19590dc310963e87a00"
dependencies = [
 "proc-macro2",
 "quote",
 "syn",
]


[[package]]
name = "sourcemap"
version = "5.0.0"
source = "registry+https://github.com/rust-lang/crates.io-index"
checksum = "8fd57aa9e5cea41b4a3c26a61039fad5585e2154ffe057a2656540a21b03a2d2"
dependencies = [
 "base64 0.10.1",
 "if_chain 0.1.3",
 "lazy_static",
 "regex",
 "rustc_version",
 "serde",
 "serde_json",
 "url 1.7.2",
]

[[package]]
name = "sourcemap"
version = "6.1.0"
source = "registry+https://github.com/rust-lang/crates.io-index"
checksum = "58ad6f449ac2dc2eaa01e766408b76b55fc0a20c842b63aa11a8448caa72f50b"
dependencies = [
 "base64 0.13.0",
 "if_chain 1.0.2",
 "lazy_static",
 "regex",
 "rustc_version",
 "serde",
 "serde_json",
 "url 2.2.2",
]
```

The final resolved version of `serde` is directly set to lock file, which means the resolver resolves the dependency at the time that lock file is generated. The reason why it only has one version is because `1.0.144` and `1.0.143` are semver compatible.
However, the dependency `sourcemap` is semver incompatible, so Cargo keeps two versions of it.