# Wallarm API Security and third-party services interaction

## What third-party services Wallarm API Security interacts with?

Wallarm API Security interacts with the following third-party services:

* GCP storage to download an actual list of IP addresses registered in [whitelisted, blacklisted, or greylisted](../user-guides/ip-lists/overview.md) countries and data centers.

    Before installing Wallarm API Security, we recommend to ensure your machine has access to [GCP storage IP addresses](https://www.gstatic.com/ipranges/goog.json).
* Tarantool feedback server (`https://feedback.tarantool.io`) to upload standard Tarantool instance data to.

    The in-memory storage Tarantool is used by the Wallarm postanalytics module deployed to your machine from the `wallarm-tarantool` package. The Tarantool storage is deployed as two instances, custom (`wallarm-tarantool`) and standard (`tarantool`). A standard instance is deployed along with a custom one by default and is not used by the Wallarm components.
    
    Wallarm uses only a custom Tarantool instance which does not send any data to `https://feedback.tarantool.io`. However, a default instance can send the data to the Tarantool feedback server once per hour ([more details](https://www.tarantool.io/en/doc/latest/reference/configuration/#feedback)).

## Can I disable sending the standard Tarantool instance data to `https://feedback.tarantool.io`?

Yes, you can disable sending the standard Tarantool instance data to `https://feedback.tarantool.io` as follows:

* If you do not use the standard Tarantool instance, you can disable it:

    ```bash
    systemctl stop tarantool
    ```
* If the standard Tarantool instance is addressing your issues, you can disable sending the data to `https://feedback.tarantool.io` using the parameter [`feedback_enabled`](https://www.tarantool.io/en/doc/latest/reference/configuration/#cfg-logging-feedback-enabled).
