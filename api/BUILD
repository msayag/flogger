# Copyright 2012 Google Inc. All Rights Reserved.
# Author: cgruber@google.com (Christian Edward Gruber)
#
# Description:
#   Flogger core libraries (google.github.io/flogger).

package(default_visibility = ["//:internal"])

CHECKS_SRCS = ["src/main/java/com/google/common/flogger/util/Checks.java"]

TAGS_SRCS = [
    "src/main/java/com/google/common/flogger/backend/KeyValueFormatter.java",
    "src/main/java/com/google/common/flogger/backend/KeyValueHandler.java",
    "src/main/java/com/google/common/flogger/backend/Tags.java",
]

LAZY_ARGS_SRCS = [
    "src/main/java/com/google/common/flogger/LazyArg.java",
    "src/main/java/com/google/common/flogger/LazyArgs.java",
]

STACK_SRCS = [
    "src/main/java/com/google/common/flogger/LogSiteStackTrace.java",
    "src/main/java/com/google/common/flogger/StackSize.java",
]

CONFIG_SRCS = ["src/main/java/com/google/common/flogger/LoggerConfig.java"]

SYSTEM_BACKEND_SRCS = glob(["src/main/java/com/google/common/flogger/backend/system/**/*.java"])

# The core 'frontend' Flogger library.
#
java_library(
    name = "api",
    srcs = glob(
        ["src/main/java/**/*.java"],
        exclude = CHECKS_SRCS +
                  TAGS_SRCS +
                  LAZY_ARGS_SRCS +
                  STACK_SRCS +
                  CONFIG_SRCS +
                  SYSTEM_BACKEND_SRCS,
    ),
    javacopts = ["-source 1.6 -target 1.6"],
    exports = [
        ":lazy_args",
        ":stack",
        ":tags",
    ],
    deps = [
        ":checks",
        ":lazy_args",
        ":platform_provider",
        ":stack",
        ":tags",
        "@google_bazel_common//third_party/java/error_prone:annotations",
        "@google_bazel_common//third_party/java/jsr305_annotations",
    ],
)

java_library(
    name = "checks",
    srcs = CHECKS_SRCS,
    javacopts = ["-source 1.6 -target 1.6"],
)

# A separate library for exposing just the 'Tags' class for use with the
# TraceLoggingContext.
#
java_library(
    name = "tags",
    srcs = TAGS_SRCS,
    javacopts = ["-source 1.6 -target 1.6"],
    deps = [
        ":checks",
        "@google_bazel_common//third_party/java/error_prone:annotations",
        "@google_bazel_common//third_party/java/jsr305_annotations",
    ],
)

# A separate library for exposing just the 'LazyArgs' classes, since these are
# support classes which can be imported and used freely in general code.
java_library(
    name = "lazy_args",
    srcs = LAZY_ARGS_SRCS,
    javacopts = ["-source 1.6 -target 1.6"],
    deps = [
        ":checks",
        "@google_bazel_common//third_party/java/jsr305_annotations",
    ],
)

# A separate library for exposing just the 'LoggerConfig' classes, since these are
# support classes which can be imported and used freely in general code.
java_library(
    name = "config",
    srcs = CONFIG_SRCS,
    javacopts = ["-source 1.6 -target 1.6"],
    deps = [
        ":api",
        ":checks",
        "@google_bazel_common//third_party/java/error_prone:annotations",
        "@google_bazel_common//third_party/java/jsr305_annotations",
    ],
)

# A separate library for exposing just the 'Stack' classes. This is exported as
# part of the core library.
java_library(
    name = "stack",
    srcs = STACK_SRCS,
    javacopts = ["-source 1.6 -target 1.6"],
    deps = [
        "@google_bazel_common//third_party/java/error_prone:annotations",
        "@google_bazel_common//third_party/java/jsr305_annotations",
    ],
)

# Core library to implement a JDK compatible backend for Flogger.
#
java_library(
    name = "system_backend",
    srcs = SYSTEM_BACKEND_SRCS,
    javacopts = ["-source 1.6 -target 1.6"],
    deps = [
        ":api",
        ":checks",
        "@google_bazel_common//third_party/java/jsr305_annotations",
    ],
)

# Utilities to help test Fluent Loggers. This target includes simple fakes for
# creating injectable test data as well as Truth subjects for asserting things
# about log statements. This is exported via @google_bazel_common//third_party/java/flogger:testing
# but should only be needed by anyone writing their own fluent logger.
java_library(
    name = "testing",
    testonly = 1,
    srcs = glob(["src/test/java/com/google/common/flogger/testing/*.java"]),
    deps = [
        ":api",
        ":checks",
        "@google_bazel_common//third_party/java/error_prone:annotations",
        "@google_bazel_common//third_party/java/guava",
        "@google_bazel_common//third_party/java/jsr305_annotations",
        "@google_bazel_common//third_party/java/junit",
        "@google_bazel_common//third_party/java/truth",
    ],
)

java_binary(
    name = "platform_provider_generator",
    srcs = ["platformprovider/main/java/com/google/common/flogger/backend/PlatformProviderGenerator.java"],
    main_class = "com.google.common.flogger.backend.PlatformProviderGenerator",
    deps = ["@google_bazel_common//third_party/java/asm"],
)

genrule(
    name = "gen_platform_provider",
    outs = ["platform_provider.jar"],
    cmd = "$(location :platform_provider_generator) com/google/common/flogger/backend/PlatformProvider.class" +
          " && $(location @local_jdk//:bin/jar) cf $@ com/google/common/flogger/backend/PlatformProvider.class",
    tools = [
        ":platform_provider_generator",
        "@local_jdk//:bin/jar",
    ],
)

java_import(
    name = "platform_provider",
    jars = ["platform_provider.jar"],
    proguard_specs = ["proguard.cfg"],
)

filegroup(
    name = "proguard_cfg",
    srcs = ["proguard.cfg"],
    visibility = [
        "//javatests/com/google/frameworks/client/logging/android/flogger/proguard:__pkg__",
    ],
)

# ---- Unit Tests ----

load("@google_bazel_common//testing:test_defs.bzl", "gen_java_tests")

gen_java_tests(
    name = "api_tests",
    srcs = glob(
        ["src/test/java/**/*.java"],
        exclude = [
            "src/test/java/com/google/common/flogger/testing/**/*.java",
        ],
    ),
    deps = [
        ":api",
        ":config",
        ":system_backend",
        ":testing",
        "@google_bazel_common//third_party/java/auto:service",
        "@google_bazel_common//third_party/java/guava",
        "@google_bazel_common//third_party/java/guava:testlib",
        "@google_bazel_common//third_party/java/jsr305_annotations",
        "@google_bazel_common//third_party/java/junit",
        "@google_bazel_common//third_party/java/mockito",
        "@google_bazel_common//third_party/java/truth",
    ],
)