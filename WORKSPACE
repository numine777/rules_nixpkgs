workspace(name = "io_tweag_rules_nixpkgs")

load(
    "//nixpkgs:nixpkgs.bzl",
    "nixpkgs_cc_configure",
    "nixpkgs_git_repository",
    "nixpkgs_local_repository",
    "nixpkgs_package",
    "nixpkgs_posix_configure",
    "nixpkgs_python_configure",
)

# For tests

nixpkgs_git_repository(
    name = "remote_nixpkgs",
    remote = "https://github.com/NixOS/nixpkgs",
    revision = "18.09",
    sha256 = "6451af4083485e13daa427f745cbf859bc23cb8b70454c017887c006a13bd65e",
)

nixpkgs_local_repository(
    name = "nixpkgs",
    nix_file = "//:nixpkgs.nix",
    nix_file_deps = ["//:nixpkgs.json"],
)

nixpkgs_package(
    name = "nixpkgs-git-repository-test",
    attribute_path = "hello",
    repositories = {"nixpkgs": "@remote_nixpkgs"},
)

nixpkgs_package(
    name = "hello",
    # Deliberately not repository, to test whether repositories works.
    repositories = {"nixpkgs": "@nixpkgs"},
)

nixpkgs_package(
    name = "expr-test",
    nix_file_content = "let pkgs = import <nixpkgs> { config = {}; overlays = []; }; in pkgs.hello",
    nix_file_deps = ["//:nixpkgs.json"],
    # Deliberately not @nixpkgs, to test whether explict file works.
    repositories = {"nixpkgs": "//:nixpkgs.nix"},
)

nixpkgs_package(
    name = "attribute-test",
    attribute_path = "hello",
    repository = "@nixpkgs",
)

nixpkgs_package(
    name = "expr-attribute-test",
    attribute_path = "hello",
    nix_file_content = "import <nixpkgs> { config = {}; overlays = []; }",
    repository = "@nixpkgs",
)

nixpkgs_package(
    name = "nix-file-test",
    attribute_path = "hello",
    nix_file = "//tests:nixpkgs.nix",
    repository = "@nixpkgs",
)

nixpkgs_package(
    name = "nix-file-deps-test",
    nix_file = "//tests:hello.nix",
    nix_file_deps = ["//tests:pkgname.nix"],
    repository = "@nixpkgs",
)

nixpkgs_package(
    name = "output-filegroup-test",
    nix_file = "//tests:output.nix",
    repository = "@nixpkgs",
)

nixpkgs_package(
    name = "extra-args-test",
    nix_file_content = """
{ packagePath }: (import <nixpkgs> { config = {}; overlays = []; }).${packagePath}
    """,
    nixopts = [
        "--argstr",
        "packagePath",
        "hello",
    ],
    repository = "@nixpkgs",
)

nixpkgs_package(
    name = "output-filegroup-manual-test",
    build_file_content = """
package(default_visibility = [ "//visibility:public" ])
filegroup(
    name = "manual-filegroup",
    srcs = glob(["hi-i-exist", "hi-i-exist-too", "bin/*"]),
)
""",
    nix_file = "//tests:output.nix",
    repository = "@nixpkgs",
)

nixpkgs_package(
    name = "relative-imports",
    attribute_path = "hello",
    nix_file = "//tests:relative_imports.nix",
    nix_file_deps = [
        "//:nixpkgs.json",
        "//:nixpkgs.nix",
        "//tests:relative_imports/nixpkgs.nix",
    ],
    repository = "@nixpkgs",
)

nixpkgs_cc_configure(repository = "@remote_nixpkgs")

nixpkgs_python_configure(repository = "@remote_nixpkgs")

load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

http_archive(
    name = "rules_sh",
    sha256 = "ad72710293db59986e1400d70944b3a13a52a11ce411969634669fc545e06c2f",
    strip_prefix = "rules_sh-5aaea6d44c1ff1ccefc9ec4a22acef301af8f90a",
    urls = ["https://github.com/tweag/rules_sh/archive/5aaea6d44c1ff1ccefc9ec4a22acef301af8f90a.tar.gz"],
)

load("@rules_sh//sh:repositories.bzl", "rules_sh_dependencies")

rules_sh_dependencies()

nixpkgs_posix_configure(repository = "@nixpkgs")

load("@rules_sh//sh:posix.bzl", "sh_posix_configure")

sh_posix_configure()
