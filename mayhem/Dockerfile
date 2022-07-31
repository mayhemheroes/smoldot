# Build Stage
FROM ghcr.io/evanrichter/cargo-fuzz:latest as builder

## Add source code to the build stage.
ADD . /src
WORKDIR /src

RUN echo building instrumented harnesses && \
    bash -c "pushd bin/fuzz && cargo +nightly -Z sparse-registry fuzz build && popd" && \
    mv bin/fuzz/target/x86_64-unknown-linux-gnu/release/chain-spec /chain-spec && \
    mv bin/fuzz/target/x86_64-unknown-linux-gnu/release/fork-tree /fork-tree && \
    mv bin/fuzz/target/x86_64-unknown-linux-gnu/release/grandpa-justification-parse /grandpa-justification-parse && \
    mv bin/fuzz/target/x86_64-unknown-linux-gnu/release/header-parse /header-parse && \
    mv bin/fuzz/target/x86_64-unknown-linux-gnu/release/json-rpc-call /json-rpc-call && \
    mv bin/fuzz/target/x86_64-unknown-linux-gnu/release/multiaddr-bytes /multiaddr-bytes && \
    mv bin/fuzz/target/x86_64-unknown-linux-gnu/release/multiaddr-text /multiaddr-text && \
    mv bin/fuzz/target/x86_64-unknown-linux-gnu/release/multihash /multihash && \
    mv bin/fuzz/target/x86_64-unknown-linux-gnu/release/network-connection-encrypted /network-connection-encrypted && \
    mv bin/fuzz/target/x86_64-unknown-linux-gnu/release/network-connection-raw /network-connection-raw && \
    mv bin/fuzz/target/x86_64-unknown-linux-gnu/release/peer-id /peer-id && \
    mv bin/fuzz/target/x86_64-unknown-linux-gnu/release/proof-node-decode /proof-node-decode && \
    mv bin/fuzz/target/x86_64-unknown-linux-gnu/release/protocol-block-announce-decode /protocol-block-announce-decode && \
    mv bin/fuzz/target/x86_64-unknown-linux-gnu/release/protocol-block-announces-handshake-decode /protocol-block-announces-handshake-decode && \
    mv bin/fuzz/target/x86_64-unknown-linux-gnu/release/protocol-blocks-request-decode /protocol-blocks-request-decode && \
    mv bin/fuzz/target/x86_64-unknown-linux-gnu/release/protocol-blocks-response-decode /protocol-blocks-response-decode && \
    mv bin/fuzz/target/x86_64-unknown-linux-gnu/release/protocol-grandpa-warp-sync-response-decode /protocol-grandpa-warp-sync-response-decode && \
    mv bin/fuzz/target/x86_64-unknown-linux-gnu/release/protocol-grandpa-notification-decode /protocol-grandpa-notification-decode && \
    mv bin/fuzz/target/x86_64-unknown-linux-gnu/release/protocol-identify-response-decode /protocol-identify-response-decode && \
    mv bin/fuzz/target/x86_64-unknown-linux-gnu/release/protocol-state-response-decode /protocol-state-response-decode && \
    mv bin/fuzz/target/x86_64-unknown-linux-gnu/release/protocol-storage-call-proof-response-decode /protocol-storage-call-proof-response-decode && \
    mv bin/fuzz/target/x86_64-unknown-linux-gnu/release/wasm-module-wasmi /wasm-module-wasmi && \
    mv bin/fuzz/target/x86_64-unknown-linux-gnu/release/wasm-module-wasmtime /wasm-module-wasmtime && \
    echo done

RUN echo building non-instrumented harnesses && \
    export RUSTFLAGS="--cfg fuzzing -Clink-dead-code -Cdebug-assertions -C codegen-units=1" && \
    bash -c "pushd bin/fuzz && cargo +nightly -Z sparse-registry build --release && popd" && \
    mv bin/fuzz/target/release/chain-spec /chain-spec_no_inst && \
    mv bin/fuzz/target/release/fork-tree /fork-tree_no_inst && \
    mv bin/fuzz/target/release/grandpa-justification-parse /grandpa-justification-parse_no_inst && \
    mv bin/fuzz/target/release/header-parse /header-parse_no_inst && \
    mv bin/fuzz/target/release/json-rpc-call /json-rpc-call_no_inst && \
    mv bin/fuzz/target/release/multiaddr-bytes /multiaddr-bytes_no_inst && \
    mv bin/fuzz/target/release/multiaddr-text /multiaddr-text_no_inst && \
    mv bin/fuzz/target/release/multihash /multihash_no_inst && \
    mv bin/fuzz/target/release/network-connection-encrypted /network-connection-encrypted_no_inst && \
    mv bin/fuzz/target/release/network-connection-raw /network-connection-raw_no_inst && \
    mv bin/fuzz/target/release/peer-id /peer-id_no_inst && \
    mv bin/fuzz/target/release/proof-node-decode /proof-node-decode_no_inst && \
    mv bin/fuzz/target/release/protocol-block-announce-decode /protocol-block-announce-decode_no_inst && \
    mv bin/fuzz/target/release/protocol-block-announces-handshake-decode /protocol-block-announces-handshake-decode_no_inst && \
    mv bin/fuzz/target/release/protocol-blocks-request-decode /protocol-blocks-request-decode_no_inst && \
    mv bin/fuzz/target/release/protocol-blocks-response-decode /protocol-blocks-response-decode_no_inst && \
    mv bin/fuzz/target/release/protocol-grandpa-warp-sync-response-decode /protocol-grandpa-warp-sync-response-decode_no_inst && \
    mv bin/fuzz/target/release/protocol-grandpa-notification-decode /protocol-grandpa-notification-decode_no_inst && \
    mv bin/fuzz/target/release/protocol-identify-response-decode /protocol-identify-response-decode_no_inst && \
    mv bin/fuzz/target/release/protocol-state-response-decode /protocol-state-response-decode_no_inst && \
    mv bin/fuzz/target/release/protocol-storage-call-proof-response-decode /protocol-storage-call-proof-response-decode_no_inst && \
    mv bin/fuzz/target/release/wasm-module-wasmi /wasm-module-wasmi_no_inst && \
    mv bin/fuzz/target/release/wasm-module-wasmtime /wasm-module-wasmtime_no_inst && \
    echo done

# Package Stage
FROM rustlang/rust:nightly

COPY --from=builder /chain-spec /chain-spec_no_inst /
COPY --from=builder /fork-tree /fork-tree_no_inst /
COPY --from=builder /grandpa-justification-parse /grandpa-justification-parse_no_inst /
COPY --from=builder /header-parse /header-parse_no_inst /
COPY --from=builder /json-rpc-call /json-rpc-call_no_inst /
COPY --from=builder /multiaddr-bytes /multiaddr-bytes_no_inst /
COPY --from=builder /multiaddr-text /multiaddr-text_no_inst /
COPY --from=builder /multihash /multihash_no_inst /
COPY --from=builder /network-connection-encrypted /network-connection-encrypted_no_inst /
COPY --from=builder /network-connection-raw /network-connection-raw_no_inst /
COPY --from=builder /peer-id /peer-id_no_inst /
COPY --from=builder /proof-node-decode /proof-node-decode_no_inst /
COPY --from=builder /protocol-block-announce-decode /protocol-block-announce-decode_no_inst /
COPY --from=builder /protocol-block-announces-handshake-decode /protocol-block-announces-handshake-decode_no_inst /
COPY --from=builder /protocol-blocks-request-decode /protocol-blocks-request-decode_no_inst /
COPY --from=builder /protocol-blocks-response-decode /protocol-blocks-response-decode_no_inst /
COPY --from=builder /protocol-grandpa-warp-sync-response-decode /protocol-grandpa-warp-sync-response-decode_no_inst /
COPY --from=builder /protocol-grandpa-notification-decode /protocol-grandpa-notification-decode_no_inst /
COPY --from=builder /protocol-identify-response-decode /protocol-identify-response-decode_no_inst /
COPY --from=builder /protocol-state-response-decode /protocol-state-response-decode_no_inst /
COPY --from=builder /protocol-storage-call-proof-response-decode /protocol-storage-call-proof-response-decode_no_inst /
COPY --from=builder /wasm-module-wasmi /wasm-module-wasmi_no_inst /
COPY --from=builder /wasm-module-wasmtime /wasm-module-wasmtime_no_inst /
