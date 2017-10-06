load("@bazel_tools//tools/build_defs/pkg:pkg.bzl", "pkg_tar")


# Use "manual" target tag to skip rules in the wildcard expansion

pkg_tar(
    name = "mixer_tar",
    extension = "tar.gz",
    files = [
        "//cmd/server:mixs",
    ],
    mode = "0755",
    package_dir = "/usr/local/bin/",
    tags = ["manual"],
)

docker_build(
    name = "mixer",
    cmd = [
        "--configStoreURL=fs:///etc/opt/mixer/configroot",
        "--configStore2URL=k8s://",
        "--logtostderr",
        "--v=2",
        "--traceOutput=http://zipkin:9411/api/v1/spans",
    ],
    entrypoint = [
        "/usr/local/bin/mixs",
        "server",
    ],
    ports = [
        "9091",
        "9093",
        "9094",
        "42422",
    ],
    repository = "istio",
    tags = ["manual"],
    tars = [
        ":mixer_tar",
        "//testdata:configs_tar",
        # ADD CERTS
    ],
)
