package(default_visibility = ["//visibility:public"])

load("@io_bazel_rules_sass//:defs.bzl", "sass_binary", "sass_library")
load("@aspect_bazel_lib//lib:expand_make_vars.bzl", "expand_template")

expand_template(
    name = "key_colors",
    out = "_key_colors.scss",
    substitutions = {
        "@SELECTION_COLOR@": "SELECTION_COLOR",
        "@ACCENT_COLOR@": "ACCENT_COLOR",
        "@SUGGESTION_COLOR@": "SUGGESTION_COLOR",
        "@DESTRUCTION_COLOR@": "DESTRUCTION_COLOR",
    },
    template = "_key_colors.scss.in",
)
