## List

The `list` subcommand for `transient` has two forms. The `list image` command
provides a way to list various properties of the images in the Disk Frontend
and Disk Backend. The `list vm` command shows information about running
`transient`-based virtual machines.

### List Image Usage

```
transient list image [-h] [-image IMG [IMG ...]] [-image-frontend IMAGE_FRONTEND]
                          [-image-backend IMAGE_BACKEND] [-name NAME]
```

- `-name NAME`: Find images associated with the provided `NAME`. Unlike the `run`
subcommand, this flag is not required for `list`. It is possible to list all VM
images associated with a given backend image by specifying only `-image` and not
`-name`, for example.

- `-image IMG [IMG ...]`: List images backed by the provided `IMG`. If no `-name`
is specified, information about the backend image itself is listed

- `-image-frontend FRONTEND`: Use the provided `FRONTEND` path as the location to
find the per-VM image copies. Note: this path defaults to
`~/.local/share/transient/frontend`.

- `-image-backend BACKEND`: Use the provided `BACKEND` path as the location to
find the read-only backing images. Note: this path defaults to
`~/.local/share/transient/backend`.

### List VM Usage

```
transient list vm [-h] [-name NAME] [-with-ssh]
```

- `-name NAME`: Find a running transient instance with the given name.
- `-with-ssh`: Only display information about virtual machines that have
an accessible SSH port


### Examples

#### List all images associated with a given VM name:
```
$ transient list image -name test-vm
Frontend Images:
┌─────────┬─────────────────────────┬──────────┬────────────┬───────────┐
│ VM Name │ Backend Image           │ Disk Num │  Real Size │ Virt Size │
├─────────┼─────────────────────────┼──────────┼────────────┼───────────┤
│ test-vm │ centos/7:2004.01        │        0 │ 110.57 MiB │ 40.00 GiB │
├─────────┼─────────────────────────┼──────────┼────────────┼───────────┤
│ test-vm │ generic/alpine38:v3.0.2 │        0 │   1.32 MiB │ 32.00 GiB │
└─────────┴─────────────────────────┴──────────┴────────────┴───────────┘
```

#### List info about the given backend image name

Note that this includes the frontend images that are backed by that image
```
$ transient list image -image centos/7:2004.01
Frontend Images:
┌─────────┬──────────────────┬──────────┬────────────┬───────────┐
│ VM Name │ Backend Image    │ Disk Num │  Real Size │ Virt Size │
├─────────┼──────────────────┼──────────┼────────────┼───────────┤
│ test-vm │ centos/7:2004.01 │        0 │ 110.57 MiB │ 40.00 GiB │
└─────────┴──────────────────┴──────────┴────────────┴───────────┘

Backend Images:
┌──────────────────┬───────────┬───────────┐
│ Image Name       │ Real Size │ Virt Size │
├──────────────────┼───────────┼───────────┤
│ centos/7:2004.01 │  1.05 GiB │ 40.00 GiB │
└──────────────────┴───────────┴───────────┘
```

#### List info about specific images associated with a VM name
```
$ transient list image -image centos/7:2004.01 -name test-vm
Frontend Images:
┌─────────┬──────────────────┬──────────┬────────────┬───────────┐
│ VM Name │ Backend Image    │ Disk Num │  Real Size │ Virt Size │
├─────────┼──────────────────┼──────────┼────────────┼───────────┤
│ test-vm │ centos/7:2004.01 │        0 │ 110.57 MiB │ 40.00 GiB │
└─────────┴──────────────────┴──────────┴────────────┴───────────┘
```

#### List info about a running VM by name
```
$ transient list vm -name test-vm
┌─────────┬────────────────────────────┬────────┬──────────┐
│ VM Name │ Start Time                 │    PID │ SSH Port │
├─────────┼────────────────────────────┼────────┼──────────┤
│ test-vm │ 2020-11-24 14:35:25.529011 │ 745004 │    58095 │
└─────────┴────────────────────────────┴────────┴──────────┘
```
