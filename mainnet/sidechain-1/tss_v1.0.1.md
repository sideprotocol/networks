## TSSigner Update

1. Update to version v1.0.1 

```
cd shuttler
git checkout v1.0.1
cargo build --release
cp target/release/shuttler ~/.cargo/bin
```

2. Set the bootstrapping nodes

```
bootstrap_nodes = ["/ip4/192.248.191.173/tcp/5158/p2p/12D3KooWQ4uVSfaPh5MKhrUoabnxm87ysutaNLjua73DjHa5uymt","/ip4/202.182.119.24/tcp/5158/p2p/12D3KooWJsCsfBUcm9yZvqbbCZ2eg8vY6Wgw2qxsafzfHNuSRhzB"]
```

3. Start as signer (**Recommended**)

```
shuttler --home <home> start --signer
```