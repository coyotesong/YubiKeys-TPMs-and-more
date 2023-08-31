# YubiKeys-TPMs-and-more

We all know we should use hardware security tokens for improved security, but there's always
been two major challenges for their widespread adoption.

- There is little documentation on how to configure and use the hardware tokens
- There is little documentation on how to configure and use them for authentication

This repo hopes to shine some light on this.

| Important: This is a preliminary work-in-progress |
+:-------------------------------------------------:+

## Hardware tokens

I'm concerned with two types of hardware tokens.

### FIDO 2 Compliant hardware tokens

These are small devices that can be kept on a keyring or left in a laptop's USB port. They
may also support NFC connectivity. They're often referred to as "YubiKeys" since that's the
most popular model but there are other manufactures. Note: Only YubiKey 5+ is FIDO 2
compliant.

Some of these devices also provide a limited amount of secured storage.

All of these devices require the user to physically touch the device.

They cost about $50+, and it's always a good idea to order at least two so you have a backup
device. Three or more is even better since you can keep one or more of them off-site. It is
also possible to get small covers for the hardware tokens you keep on a keyring - it's $10
well-spent since the tokens can break if subjected to enough force.

### Trusted Compute Modules (TPM)

Any PC motherboard since 2016 or so should either have a TPM installed, or at least the
required header for a compatible TPM. They run about $20 but they are not interchangable -
you need to be sure you ordered the correct module for your motherboard.

You do not need to physically touch the TPM - that would be impossible since it's located
on the motherboard. You **may** need to enter a PIN in your application.

## Applications

#### SSH

The most common use for hardware tokens is to use one when using RSA-based authentication
to an SSH server. (Nobody uses the basic username/password authentication anymore, right?)

There's two major problems in practice:

- few people use a passphrase on their private key. Anyone who can get a copy of the private key can impersonate the user.
- the passphrase is also vulnerable if a malicious actor has installed a key logger.

A YubiKey solves this by removing the ability of an attacker to exfiltrate the private key.
All it requires is a FIDO 2 token and a small change to `ssh-keygen`

```sh
$ ssh-keygen -t ed25519_sk
```

Note: you can create your private key in the traditional way and then import it into the hardware
token but it's considered best practices to have the hardware device generate the private key itself.



