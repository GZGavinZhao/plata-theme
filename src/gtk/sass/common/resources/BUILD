load("//utils:utils.bzl", "expand_template")
load("@io_bazel_rules_sass//:defs.bzl", "sass_library", "sass_binary")

expand_template(
	name = "key_colors",
	out = "_key_colors.scss",
	substitutions = {
		"@SELECTION_COLOR@": "bruh",
	},
	template = "_key_colors.scss.in",
)
