load("@io_bazel_rules_kotlin//kotlin:jvm.bzl", "kt_jvm_library", "kt_jvm_test", "kt_jvm_binary")
load("@rules_java//java:defs.bzl", "java_proto_library")
load("@rules_proto//proto:defs.bzl", "proto_library")
load("//:protobuf_version.bzl", "PROTOBUF_VERSION")

# Kotlin generated protos depend on this and only this.
kt_jvm_library(
    name = "shared_runtime",
    srcs = [
        "src/main/kotlin/com/google/protobuf/DslList.kt",
        "src/main/kotlin/com/google/protobuf/DslMap.kt",
        "src/main/kotlin/com/google/protobuf/DslProxy.kt",
        "src/main/kotlin/com/google/protobuf/ExtensionList.kt",
        "src/main/kotlin/com/google/protobuf/ProtoDslMarker.kt",
        "src/main/kotlin/com/google/protobuf/UnmodifiableCollections.kt",
    ],
    visibility = ["//visibility:public"],
    deps = [
        ":only_for_use_in_proto_generated_code_its_generator_and_tests",
        "//java/lite",
    ],
)

kt_jvm_library(
    name = "only_for_use_in_proto_generated_code_its_generator_and_tests",
    srcs = ["src/main/kotlin/com/google/protobuf/OnlyForUseByGeneratedProtoCode.kt"],
    visibility = ["//java:__subpackages__"],
)

kt_jvm_library(
    name = "bytestring_lib",
    srcs = ["src/main/kotlin/com/google/protobuf/ByteStrings.kt"],
    deps = ["//java/lite"],
)

kt_jvm_library(
    name = "full_extensions",
    srcs = ["src/main/kotlin/com/google/protobuf/ExtendableMessageExtensions.kt"],
    deps = ["//java/core"],
)

test_suite(
    name = "tests",
    tests = [
        "bytestring_test",
        "shared_tests",
        "test_extensions",
        "proto2_test",
        "proto3_test",
    ],
)

kt_jvm_test(
    name = "bytestring_test",
    srcs = ["src/test/kotlin/com/google/protobuf/ByteStringsTest.kt"],
    deps = [
        ":bytestring_lib",
        "//java/lite",
        "@com_github_jetbrains_kotlin//:kotlin-test",
        "@maven//:com_google_truth_truth",
        "@maven//:junit_junit",
    ],
    test_class = "com.google.protobuf.kotlin.ByteStringsTest",
)

proto_library(
    name = "example_extensible_message_proto",
    srcs = ["src/test/proto/com/google/protobuf/example_extensible_message.proto"],
    visibility = ["//java:__subpackages__"],
)

java_proto_library(
    name = "example_extensible_message_java_proto",
    deps = [":example_extensible_message_proto"],
)

kt_jvm_test(
    name = "shared_tests",
    srcs = [
        "src/test/kotlin/com/google/protobuf/DslListTest.kt",
        "src/test/kotlin/com/google/protobuf/DslMapTest.kt",
        "src/test/kotlin/com/google/protobuf/ExtensionListTest.kt",
    ],
    deps = [
        ":bytestring_lib",
        ":example_extensible_message_java_proto",
        ":only_for_use_in_proto_generated_code_its_generator_and_tests",
        ":shared_runtime",
        "@com_github_jetbrains_kotlin//:kotlin-test",
        "@maven//:com_google_truth_truth",
        "@maven//:com_google_guava_guava_testlib",
        "@maven//:junit_junit",
    ],
    test_class = "com.google.protobuf.kotlin.DslListTest",
)

kt_jvm_test(
    name = "test_extensions",
    srcs = ["src/test/kotlin/com/google/protobuf/ExtendableMessageExtensionsTest.kt"],
    deps = [
        ":example_extensible_message_java_proto",
        ":full_extensions",
        "//java/lite",
        ":only_for_use_in_proto_generated_code_its_generator_and_tests",
        ":shared_runtime",
        "@com_github_jetbrains_kotlin//:kotlin-test",
        "@maven//:com_google_truth_truth",
        "@maven//:junit_junit",
    ],
    test_class = "com.google.protobuf.kotlin.ExtendableMessageExtensionsTest",
)

proto_library(
    name = "evil_names_proto2",
    srcs = ["src/test/proto/com/google/protobuf/evil_names_proto2.proto"],
    visibility = ["//:__subpackages__"],
)

proto_library(
    name = "evil_names_proto3",
    srcs = ["src/test/proto/com/google/protobuf/evil_names_proto3.proto"],
    visibility = ["//:__subpackages__"],
)

java_proto_library(
    name = "evil_names_proto2_java_proto",
    deps = [":evil_names_proto2"],
)

java_proto_library(
    name = "evil_names_proto3_java_proto",
    deps = [":evil_names_proto3"],
)

proto_library(
    name = "multiple_files_proto3",
    srcs = ["src/test/proto/com/google/protobuf/multiple_files_proto3.proto"],
    visibility = ["//:__subpackages__"],
)

java_proto_library( name = "multiple_files_proto3_java_proto",
    deps = [":multiple_files_proto3"],
)

genrule(
    name = "gen_kotlin_proto3_java_multiple_files",
    srcs = ["src/test/proto/com/google/protobuf/multiple_files_proto3.proto"],
    outs = [
        "MultipleFilesMessageAKt.kt",
        "MultipleFilesMessageBKt.kt",
        "MultipleFilesProto3Kt.kt",
    ],
    cmd = "$(location //:protoc) " +
          "--kotlin_out=shared,immutable:$(@D) " +
          "$(location src/test/proto/com/google/protobuf/multiple_files_proto3.proto) && " +
          "cp $(@D)/com/google/protobuf/kotlin/generator/MultipleFilesMessageAKt.kt " +
          "$(location MultipleFilesMessageAKt.kt) && " +
          "cp $(@D)/com/google/protobuf/kotlin/generator/MultipleFilesMessageBKt.kt " +
          "$(location MultipleFilesMessageBKt.kt) && " +
          "cp $(@D)/com/google/protobuf/kotlin/generator/MultipleFilesProto3Kt.kt " +
          "$(location MultipleFilesProto3Kt.kt)",
    tools = ["//:protoc"],
)

genrule(
    name = "gen_evil_names_proto2",
    srcs = ["src/test/proto/com/google/protobuf/evil_names_proto2.proto"],
    outs = [
        "EvilNamesProto2Kt.kt",
        "HardKeywordsAllTypesProto2Kt.kt",
        "InterfaceKt.kt",
    ],
    cmd = "$(location //:protoc) " +
          "--kotlin_out=shared,immutable:$(@D) " +
          "$(location src/test/proto/com/google/protobuf/evil_names_proto2.proto) && " +
          "cp $(@D)/com/google/protobuf/kotlin/generator/EvilNamesProto2Kt.kt " +
          "$(location EvilNamesProto2Kt.kt) && " +
          "cp $(@D)/com/google/protobuf/kotlin/generator/HardKeywordsAllTypesProto2Kt.kt " +
          "$(location HardKeywordsAllTypesProto2Kt.kt) && " +
          "cp $(@D)/com/google/protobuf/kotlin/generator/InterfaceKt.kt " +
          "$(location InterfaceKt.kt)",
    tools = ["//:protoc"],
)

genrule(
    name = "gen_evil_names_proto3",
    srcs = ["src/test/proto/com/google/protobuf/evil_names_proto3.proto"],
    outs = [
        "ClassKt.kt",
        "EvilNamesProto3Kt.kt",
        "HardKeywordsAllTypesProto3Kt.kt",
    ],
    cmd = "$(location //:protoc) " +
          "--kotlin_out=shared,immutable:$(@D) " +
          "$(location src/test/proto/com/google/protobuf/evil_names_proto3.proto) && " +
          "cp $(@D)/com/google/protobuf/kotlin/generator/ClassKt.kt " +
          "$(location ClassKt.kt) && " +
          "cp $(@D)/com/google/protobuf/kotlin/generator/EvilNamesProto3Kt.kt " +
          "$(location EvilNamesProto3Kt.kt) && " +
          "cp $(@D)/com/google/protobuf/kotlin/generator/HardKeywordsAllTypesProto3Kt.kt " +
          "$(location HardKeywordsAllTypesProto3Kt.kt)",
    tools = ["//:protoc"],
)

kt_jvm_library(
    name = "kotlin_unittest",
    srcs = [
        ":gen_evil_names_proto2",
        "//:gen_kotlin_unittest",
    ],
    deps = [
        ":evil_names_proto2_java_proto",
        "//java/core:core",
        ":only_for_use_in_proto_generated_code_its_generator_and_tests",
        ":shared_runtime",
        "//:java_test_protos",
    ],
)

kt_jvm_library(
    name = "kotlin_proto3_unittest",
    srcs = [
        ":gen_evil_names_proto3",
        ":gen_kotlin_proto3_java_multiple_files",
        "//:gen_kotlin_proto3_unittest",
    ],
    deps = [
        ":evil_names_proto3_java_proto",
        ":multiple_files_proto3_java_proto",
        "//java/core:core",
        ":only_for_use_in_proto_generated_code_its_generator_and_tests",
        ":shared_runtime",
        "//:java_test_protos",
    ],
)

kt_jvm_test(
    name = "proto2_test",
    srcs = ["src/test/kotlin/com/google/protobuf/Proto2Test.kt"],
    deps = [
        ":kotlin_unittest",
        "//java/core:test_util",
        "@maven//:com_google_truth_truth",
        "@maven//:junit_junit",
    ],
    test_class = "com.google.protobuf.kotlin.Proto2Test",
)

kt_jvm_test(
    name = "proto3_test",
    srcs = ["src/test/kotlin/com/google/protobuf/Proto3Test.kt"],
    deps = [
        ":kotlin_proto3_unittest",
        "//java/core:test_util",
        "@maven//:com_google_truth_truth",
        "@maven//:junit_junit",
    ],
    test_class = "com.google.protobuf.kotlin.Proto3Test",
)
