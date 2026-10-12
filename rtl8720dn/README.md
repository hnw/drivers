# RTL8720DN Driver

This package provides a driver to use a separate connected WiFi processor `RTL8720DN` for TCP/UDP communication.

## Using th RTL8720DN Driver

For now, it is only available for the `RTL8720DN` on `Wio Terminal`.
You can try the following command.

```
$ tinygo flash --target wioterminal --size short ./examples/net/webclient/
$ tinygo flash --target wioterminal --size short ./examples/net/tlsclient/
```

## RTL8720DN Firmware

Follow the steps below to update.
The firmware must be version 2.1.2 or later.

https://wiki.seeedstudio.com/Wio-Terminal-Network-Overview/


## CA certificates

Use `SetRootCA` to enable server certificate verification for TLS connections on RTL8720DN.

```go
import _ "embed"

//go:embed rootca.pem
var rootCA string

// Configure the device before opening TLS sockets.
if err := device.SetRootCA(rootCA); err != nil {
    panic(err)
}
```

`device` is the RTL8720DN device returned by `New`. The setting applies to newly created TLS sockets; existing sockets are unaffected. An empty string disables certificate verification for subsequent sockets.

The PEM data may contain multiple concatenated CA certificates, up to **4080 bytes in total**. For example, to obtain Amazon Root CA 1 for connecting to `httpbin.org`:

```sh
curl -fL https://www.amazontrust.com/repository/AmazonRootCA1.pem -o rootca.pem
```

**Note:** Some Seeed firmware versions have known TLS certificate memory-management issues. See the [firmware source](https://github.com/Seeed-Studio/seeed-ambd-firmware/blob/86119d870213a441497ac76b70b432ddd0beae6a/src/wifi/wifi_ssl_client.c) for details.
