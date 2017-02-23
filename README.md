# jetcd

[![Build Status](https://travis-ci.org/coreos/jetcd.svg?branch=master)](https://travis-ci.org/coreos/jetcd)

Java client for etcd v3.

## Getting started

### Usage

```
EtcdClient client = EtcdClientBuilder.newBuilder().endpoints("http://localhost:2379").build();
EtcdKV kvClient = client.getKVClient();

ByteString key = ByteString.copyFrom("test_key", "UTF-8");
ByteString value = ByteString.copyFrom("test_value", "UTF-8");

// put the key-value
kvClient.put(key, value).get();
// get the value
ListenableFuture<RangeResponse> getFeature = kvClient.get(key);
RangeResponse response = getFeature.get();
assertEquals(response.getKvsCount(), 1);
assertEquals(response.getKvs(0).getValue().toStringUtf8(), "test_value");
// delete the key
kvClient.delete(key).get()
```

For full etcd v3 API, plesase refer to [API_Reference](https://github.com/coreos/etcd/blob/master/Documentation/dev-guide/api_reference_v3.md).

## Running tests

The project is to be tested against a three node `etcd` setup, launched by the [scripts/run_etcd.sh](scripts/run_etcd.sh) shell script:

```
./scripts/run_etcd.sh
```

It should work on either macOS or Linux.

Note: Make sure you don't have a default `etcd` running on your system! The script uses the default port `2379` for the first node, which fails to launch if that port is already taken.

```
mvn test
...

-------------------------------------------------------
 T E S T S
-------------------------------------------------------
Running TestSuite
Feb 23, 2017 6:40:18 PM io.grpc.internal.ManagedChannelImpl <init>
INFO: [ManagedChannelImpl@48a242ce] Created with target etcd
Feb 23, 2017 6:40:18 PM io.grpc.internal.ManagedChannelImpl <init>
INFO: [ManagedChannelImpl@20d3d15a] Created with target etcd
Feb 23, 2017 6:40:18 PM io.grpc.internal.ManagedChannelImpl <init>
INFO: [ManagedChannelImpl@548a102f] Created with target etcd
Feb 23, 2017 6:40:18 PM io.grpc.internal.ManagedChannelImpl <init>
INFO: [ManagedChannelImpl@17c386de] Created with target etcd
Feb 23, 2017 6:40:19 PM io.grpc.internal.ManagedChannelImpl <init>
INFO: [ManagedChannelImpl@25d250c6] Created with target etcd
Feb 23, 2017 6:40:27 PM io.grpc.internal.ManagedChannelImpl <init>
INFO: [ManagedChannelImpl@3eb7fc54] Created with target etcd
Feb 23, 2017 6:40:27 PM io.grpc.internal.ManagedChannelImpl <init>
INFO: [ManagedChannelImpl@6ef888f6] Created with target etcd
Feb 23, 2017 6:40:33 PM io.grpc.internal.ManagedChannelImpl <init>
INFO: [ManagedChannelImpl@49b0b76] Created with target etcd
Feb 23, 2017 6:40:33 PM io.grpc.internal.ManagedChannelImpl <init>
INFO: [ManagedChannelImpl@6f96c77] Created with target etcd
Tests run: 37, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 16.797 sec - in TestSuite

Results :

Tests run: 37, Failures: 0, Errors: 0, Skipped: 0

[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 39.229 s
[INFO] Finished at: 2017-02-23T18:40:34+02:00
[INFO] Final Memory: 37M/587M
[INFO] ------------------------------------------------------------------------
```

## Contact

* Mailing list: [etcd-dev](https://groups.google.com/forum/?hl=en#!forum/etcd-dev)

## License

jetcd is under the Apache 2.0 license. See the [LICENSE](https://github.com/coreos/jetcd/blob/master/LICENSE) file for details.
