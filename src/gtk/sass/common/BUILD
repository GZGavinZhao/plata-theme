package(default_visibility = ["//visibility:public"])

load("@bazel_skylib//rules:expand_template.bzl", "expand_template")
load("@io_bazel_rules_sass//:defs.bzl", "sass_library", "sass_binary")

expand_template(
	name = "key_colors",
	out = "resources/_key_colors.scss",
	substitutions = {
		"@SELECTION_COLOR@": "bruh",
	},
	template = "resources/_key_colors.scss.in"
)

sass_library(
    name = "shared",
    srcs = [
        "_3rd-party.scss",
        "_common.scss",
	"src/gtk/sass/common:key_colors",
    ],
)


