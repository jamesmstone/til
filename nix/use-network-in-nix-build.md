# Using network in a nix build

You can **not** normally use network in a nix build, as it is not sandboxed and is therefore a source of non reproducibility.

However, with `lib.fetchers.withNormalizedHash` you can use the network so long as you provide a hash 

A simplified (by me) example from: [nixkpgs](https://github.com/NixOS/nixpkgs/blob/ff214e9d0e5e89be6e1fd5bb2d9f3add6c7c5fbb/pkgs/build-support/docker/default.nix#L141-193)
```nix
{
  addDockerImageToNixStore = let 
        defaultArchitecture = pkgs.go.GOARCH;
    in
    lib.fetchers.withNormalizedHash { } (
      { imageName
      , imageDigest
      , outputHash
      , outputHashAlgo
      , os ? "linux"
      , arch ? defaultArchitecture
      , tlsVerify ? true
      , name ? imageName
      }: pkgs.runCommand name
        {
          inherit imageDigest;
          impureEnvVars = lib.fetchers.proxyImpureEnvVars;

          inherit outputHash outputHashAlgo;
          outputHashMode = "nar";

          nativeBuildInputs = [ pkgs.skopeo pkgs.umoci ];
          SSL_CERT_FILE = "${pkgs.cacert.out}/etc/ssl/certs/ca-bundle.crt";
          sourceURL = "docker://${imageName}@${imageDigest}";
        }
        ''
          tmp="$(mktemp -d)"
          skopeo \
           --insecure-policy \
           --tmpdir="$TMPDIR" \
           --override-os "${os}" \
           --override-arch "${arch}" \
           copy \
           --src-tls-verify="${lib.boolToString tlsVerify}" \
           "$sourceURL" "oci://$tmp:latest" \
           | cat
           umoci raw unpack --rootless --image "$tmp" "$out"
        ''
    );
}
```
