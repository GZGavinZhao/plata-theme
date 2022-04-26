workspace(name = "plata-theme")

load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")
load("@bazel_tools//tools/build_defs/repo:git.bzl", "git_repository")

# Install tool
http_archive(
    name = "com_github_google_rules_install",
    sha256 = "ac2c9c53aa022a110273c0e510d191a4c04c6adafefa069a5eeaa16313edc9b9",
    strip_prefix = "bazel_rules_install-0.4",
    urls = [
        "https://github.com/google/bazel_rules_install/releases/download/0.4/bazel_rules_install-0.4.tar.gz",
    ],
)

load("@com_github_google_rules_install//:deps.bzl", "install_rules_dependencies")

install_rules_dependencies()

load("@com_github_google_rules_install//:setup.bzl", "install_rules_setup")

install_rules_setup()

# NodeJS
load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

http_archive(
    name = "build_bazel_rules_nodejs",
    sha256 = "2b2004784358655f334925e7eadc7ba80f701144363df949b3293e1ae7a2fb7b",
    urls = ["https://github.com/bazelbuild/rules_nodejs/releases/download/5.4.0/rules_nodejs-5.4.0.tar.gz"],
)

load("@build_bazel_rules_nodejs//:repositories.bzl", "build_bazel_rules_nodejs_dependencies")

build_bazel_rules_nodejs_dependencies()

load("@build_bazel_rules_nodejs//:index.bzl", "node_repositories")

node_repositories()

# Sass
http_archive(
    name = "io_bazel_rules_sass",
    sha256 = "4b6153ae6c454c5bd44f3e47e5430f8a634d4affb983a237d21ca6199e13991c",
    strip_prefix = "rules_sass-1.50.1",
    url = "https://github.com/bazelbuild/rules_sass/archive/1.50.1.zip",
)

load("@io_bazel_rules_sass//:defs.bzl", "sass_repositories")

sass_repositories()

# Skylib
http_archive(
    name = "bazel_skylib",
    sha256 = "f7be3474d42aae265405a592bb7da8e171919d74c16f082a5457840f06054728",
    urls = [
        "https://mirror.bazel.build/github.com/bazelbuild/bazel-skylib/releases/download/1.2.1/bazel-skylib-1.2.1.tar.gz",
        "https://github.com/bazelbuild/bazel-skylib/releases/download/1.2.1/bazel-skylib-1.2.1.tar.gz",
    ],
)

load("@bazel_skylib//:workspace.bzl", "bazel_skylib_workspace")

bazel_skylib_workspace()
