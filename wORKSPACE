# Declares that this directory is the root of a Bazel workspace.
# See https://docs.bazel.build/versions/master/build-ref.html#workspace
workspace(
    # How this workspace would be referenced with absolute labels from another workspace
    name = "bazel_npm_dependency_issue",
    # Map the @npm bazel workspace to the node_modules directory.
    # This lets Bazel use the same node_modules as other local tooling.
    managed_directories = {"@npm": ["node_modules"]},
)

# Install the nodejs "bootstrap" package
# This provides the basic tools for running and packaging nodejs programs in Bazel
load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")
http_archive(
    name = "build_bazel_rules_nodejs",
    sha256 = "f9e7b9f42ae202cc2d2ce6d698ccb49a9f7f7ea572a78fd451696d03ef2ee116",
    urls = ["https://github.com/bazelbuild/rules_nodejs/releases/download/1.6.0/rules_nodejs-1.6.0.tar.gz"],
)

# The npm_install rule runs yarn anytime the package.json or package-lock.json file changes.
# It also extracts any Bazel rules distributed in an npm package.
load("@build_bazel_rules_nodejs//:index.bzl",  "node_repositories", "npm_install")

node_repositories(
  node_version = "13.2.0",
  node_repositories = {
    "13.2.0-darwin_amd64": ("node-v13.2.0-darwin-x64.tar.gz", "node-v13.2.0-darwin-x64", "2bcba358ef68ea21655728126c678063c60119e18e65d04f615d6b22dba8f7a5"),
    "13.2.0-linux_amd64": ("node-v13.2.0-linux-x64.tar.xz", "node-v13.2.0-linux-x64", "366df8a38b522a5899c3f48d8c9e359b3370495cf84867b2673dc10483adbdef"),
  },
  node_urls = ["https://nodejs.org/dist/v{version}/{filename}"],
  package_json = ["//:package.json"]
)

npm_install(
    # Name this npm so that Bazel Label references look like @npm//package
    name = "npm",
    package_json = "//:package.json",
    package_lock_json = "//:package-lock.json"
)

# Install any Bazel rules which were extracted earlier by the npm_install rule.
load("@npm//:install_bazel_dependencies.bzl", "install_bazel_dependencies")
install_bazel_dependencies()

# Set up TypeScript toolchain 
load("@npm_bazel_typescript//:index.bzl", "ts_setup_workspace")
ts_setup_workspace()

