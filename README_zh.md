# ZMK Shield Cornix

这是一个基于42键Corne键盘的ZMK固件分支。

## Bootloader恢复说明

--原版RMK固件移除了SoftDevice，因此在刷新`zmk.uf2`之前，需要先恢复SoftDevice。具体步骤请参考[bootloader/README.md](./bootloader/README.md)。 --

不再需要, 已经使用 no-sd 的分区格式.

##

## 如何将Cornix Shield添加到已有ZMK项目

对于已有 zmk-config 的用户，应通过 west.yml 添加此仓库依赖并通过 west update 拉取最新版本：

### 1. 修改 west.yml

编辑 `config/west.yml` 文件，在 `manifest/remotes` 部分添加：

```yaml
remotes:
  - name: zmkfirmware
    url-base: https://github.com/zmkfirmware
  - name: cornix-shield
    url-base: https://github.com/hitsmaxft
```

在 `manifest/projects` 部分添加：

```yaml
projects:
  - name: zmk
    remote: zmkfirmware
    revision: main
    import: app/west.yml
  - name: zmk-keyboard-cornix
    remote: cornix-shield
    revision: main
```

### 2. 更新依赖

```bash
west update
```

### 3. 配置构建

编辑 `build.yaml` 文件，添加：

```yaml
include:
  - board: cornix_e73
    shield: cornix_main_left
    artifact-name: cornix_left

  - board: cornix_e73
    shield: cornix_right
    artifact-name: cornix_right

```

### 4. 构建固件

使用你喜欢的方式构建即可

### 5. 刷入固件

将生成的`.uf2`文件刷入对应的microcontroller：
- 左半部分：`cornix_left_main.uf2`
- 左半部分：`cornix_right.uf2`
