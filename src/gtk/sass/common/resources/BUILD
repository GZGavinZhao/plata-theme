package(default_visibility = ["//visibility:public"])

load("@io_bazel_rules_sass//:defs.bzl", "sass_library")
load("@aspect_bazel_lib//lib:expand_make_vars.bzl", "expand_template")
load("@aspect_bazel_lib//lib:write_source_files.bzl", "write_source_files")

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

write_source_files(
	name = "write_key_colors",
	files = {
		"_key_colors.scss": ":key_colors",
	},
)

expand_template(
    name = "mate_flag",
    out = "_mate_flag.scss",
    substitutions = {
        "@MATE_SUPPORT@": "MATE",
    },
    template = "_mate_flag.scss.in",
)

write_source_files(
	name = "write_mate_flag",
	files = {
		"_mate_flag.scss": ":mate_flag",
	},
)

sass_library(
	name = "resources",
	srcs = glob(["*.scss"]),
)
