package(
    default_visibility = [
        "//tensorflow:internal",
        "//tensorflow_models:__subpackages__",
    ],
)

licenses(["notice"])  # Apache 2.0

load(
    "//tensorflow:tensorflow.bzl",
    "tf_cc_test",
    "tf_gpu_library",
)

tf_gpu_library(
    name = "eager_executor",
    srcs = [
        "eager_executor.cc",
    ],
    hdrs = [
        "eager_executor.h",
    ],
    visibility = ["//tensorflow:internal"],
    deps = select({
        "//tensorflow:android": [
            "//tensorflow/core:android_tensorflow_lib_lite",
        ],
        "//conditions:default": [
            "//tensorflow/core:core_cpu_lib",
            "//tensorflow/core:framework",
            "//tensorflow/core:framework_internal",
            "//tensorflow/core:lib",
            "//tensorflow/core:lib_internal",
            "//tensorflow/core:protos_all_cc",
        ],
    }),
)

tf_gpu_library(
    name = "context",
    srcs = [
        "context.cc",
    ],
    hdrs = [
        "context.h",
    ],
    visibility = ["//tensorflow:internal"],
    deps = [
        ":eager_executor",
        ":kernel_and_device",
    ] + select({
        "//tensorflow:android": [
            "//tensorflow/core:android_tensorflow_lib_lite",
        ],
        "//conditions:default": [
            "//tensorflow/core:core_cpu_lib",
            "//tensorflow/core:framework",
            "//tensorflow/core:framework_internal",
            "//tensorflow/core:lib",
            "//tensorflow/core:lib_internal",
            "//tensorflow/core:protos_all_cc",
            "//tensorflow/core:session_options",
            "//tensorflow/core/distributed_runtime:server_lib",
            "//tensorflow/core/distributed_runtime:worker_session",
            "//tensorflow/core/distributed_runtime/eager:eager_client",
        ],
    }),
)

tf_gpu_library(
    name = "eager_operation",
    srcs = [
        "eager_operation.cc",
    ],
    hdrs = [
        "eager_operation.h",
    ],
    visibility = ["//tensorflow:internal"],
    deps = [
        ":attr_builder",
        ":context",
        ":tensor_handle",
    ],
)

tf_gpu_library(
    name = "tensor_handle",
    srcs = [
        "tensor_handle.cc",
    ],
    hdrs = [
        "tensor_handle.h",
    ],
    visibility = ["//tensorflow:internal"],
    deps = [
        ":context",
        ":eager_executor",
        ":kernel_and_device",
    ] + select({
        "//tensorflow:android": [
            "//tensorflow/core:android_tensorflow_lib_lite",
        ],
        "//conditions:default": [
            "//tensorflow/core:core_cpu_lib",
            "//tensorflow/core:framework",
            "//tensorflow/core:framework_internal",
            "//tensorflow/core:lib",
            "//tensorflow/core:lib_internal",
            "//tensorflow/core:protos_all_cc",
            "//tensorflow/core:session_options",
        ],
    }),
)

tf_gpu_library(
    name = "copy_to_device_node",
    hdrs = [
        "copy_to_device_node.h",
    ],
    visibility = ["//tensorflow:internal"],
    deps = [
        ":context",
        ":eager_executor",
        ":tensor_handle",
    ] + select({
        "//tensorflow:android": [
        ],
        "//conditions:default": [
            "//tensorflow/core:core_cpu_lib",
            "//tensorflow/core:framework",
            "//tensorflow/core:framework_internal",
            "//tensorflow/core:lib",
            "//tensorflow/core:lib_internal",
            "//tensorflow/core:protos_all_cc",
            "//tensorflow/core:session_options",
        ],
    }),
)

tf_gpu_library(
    name = "kernel_and_device",
    srcs = [
        "kernel_and_device.cc",
    ],
    hdrs = [
        "kernel_and_device.h",
    ],
    visibility = ["//tensorflow:internal"],
    deps = select({
        "//tensorflow:android": [
            "//tensorflow/core:android_tensorflow_lib_lite",
            "//util/hash:farmhash_fingerprint",
        ],
        "//conditions:default": [
            "//tensorflow/core:core_cpu_lib",
            "//tensorflow/core:framework",
            "//tensorflow/core:framework_internal",
            "//tensorflow/core:lib",
            "//tensorflow/core:lib_internal",
            "//tensorflow/core:protos_all_cc",
        ],
    }),
)

tf_cc_test(
    name = "kernel_and_device_test",
    srcs = ["kernel_and_device_test.cc"],
    deps = [
        ":attr_builder",
        ":kernel_and_device",
        "//tensorflow/cc:cc_ops",
        "//tensorflow/cc:client_session",
        "//tensorflow/cc:ops",
        "//tensorflow/cc:scope",
        "//tensorflow/core:core_cpu",
        "//tensorflow/core:core_cpu_internal",
        "//tensorflow/core:framework",
        "//tensorflow/core:lib",
        "//tensorflow/core:test",
        "//tensorflow/core:test_main",
    ],
)

cc_library(
    name = "execute",
    srcs = ["execute.cc"],
    hdrs = [
        "execute.h",
        "execute_node.h",
    ],
    deps = [
        ":context",
        ":copy_to_device_node",
        ":eager_executor",
        ":eager_operation",
        ":kernel_and_device",
        ":tensor_handle",
    ] + select({
        "//tensorflow:android": [
            "//tensorflow/core:android_tensorflow_lib_lite",
        ],
        "//conditions:default": [
            "//tensorflow/core:core_cpu_lib",
            "//tensorflow/core:framework",
            "//tensorflow/core:lib",
            "//tensorflow/core:lib_internal",
            "//tensorflow/core:protos_all_cc",
            "//tensorflow/core/distributed_runtime/eager:eager_client",
            "//tensorflow/core/distributed_runtime/eager:remote_execute_node",
        ],
    }),
)

tf_gpu_library(
    name = "attr_builder",
    srcs = ["attr_builder.cc"],
    hdrs = ["attr_builder.h"],
    visibility = ["//tensorflow:internal"],
    deps = [
        ":kernel_and_device",
        "//tensorflow/c:c_api",
    ] + select({
        "//tensorflow:android": [
            "//tensorflow/core:android_tensorflow_lib_lite",
            "//util/hash:farmhash_fingerprint",
        ],
        "//conditions:default": [
            "//tensorflow/core:core_cpu",
            "//tensorflow/core:core_cpu_internal",
            "//tensorflow/core:framework",
            "//tensorflow/core:framework_internal",
            "//tensorflow/core:lib",
            "//tensorflow/core:lib_internal",
            "//tensorflow/core:protos_all_cc",
        ],
    }),
)

tf_cc_test(
    name = "attr_builder_test",
    srcs = ["attr_builder_test.cc"],
    deps = [
        ":attr_builder",
        "//tensorflow/cc:cc_ops",
        "//tensorflow/cc:client_session",
        "//tensorflow/cc:ops",
        "//tensorflow/cc:scope",
        "//tensorflow/core:core_cpu_internal",
        "//tensorflow/core:framework",
        "//tensorflow/core:lib",
        "//tensorflow/core:test",
        "//tensorflow/core:test_main",
    ],
)
