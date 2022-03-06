# NetBSDインストールメディアについてのメモ

## インストーラのルートファイルシステムの作成について

`src/distrib/common/bootimage/Makefile.bootimage`

```Makefile
#
# definitions to create root fs
#
SETS_DEFAULT=   modules base etc comp games gpufw man misc rescue tests text
.if ${MKX11} != "no"
SETS_DEFAULT+=  xbase xcomp xetc xfont xserver
.endif
```

最終的にこのMakefileの中で、カーネルのアーカイブと一緒に `IMG_SETS` 変数へまとめられる。

これらのアーカイブはインストーラ作成時点で変数 `SETS_DIR` 直下に配置されていることが期待されている。 `SETS_DIR` はデフォルトでは `${RELEASEDIR}/${RELEASEMACHINEDIR}/binary/sets` 、つまり `build.sh` でsetsを作ったときにアーカイブが配置されるディレクトリを指している。

```Makefile
#
# create root file system for the image
#
${TARGETFS}: prepare_md_post ${WORKFSTAB}
    @if [ ! -d ${RELEASEDIR}/${RELEASEMACHINEDIR} ]; then       \
        echo "Missing ${RELEASEDIR}/${RELEASEMACHINEDIR}, aborting"; \
        false;                          \
    fi;
    @${MKDIR} ${MKDIRPERM} ${WORKDIR}
.for set in ${IMG_SETS}
    @if [ ! -f ${SETS_DIR}/${set}.${TAR_SUFF} ]; then       \
        echo "Missing ${SETS_DIR}/${set}.${TAR_SUFF}, aborting";\
        false;                          \
    fi
    @echo Extracting ${set}.${TAR_SUFF} ...
    @(cd ${WORKDIR}; ${TOOL_PAX} ${PAX_TIMESTAMP} -rn \
        --use-compress-program=${COMPRESS_PROGRAM:Q} \
        -f ${SETS_DIR}/${set}.${TAR_SUFF} .)
.endfor
```

## インストーラへのsetsコピーについて

変数 `IMGDIR_EXTRA` を定義することでブート可能なメディアを作る時に追加でそのメディアにコピーできる。

```Makefile
#   IMGDIR_EXTRA
#       list of additional directories to be copied into images,
#       containing one or more tuples of the form:
#           DIR TARGETPATH
#       for installation image etc.
#       (default: empty)
#       XXX: currently permissions in IMGDIR_EXTRA are not handl
```

`src/distrib/common/bootimage/Makefile.installimage` では次のように定義している。

```Makefile
IMGDIR_EXTRA=   ${RELEASEDIR}/${RELEASEMACHINEDIR}  ${RELEASEMACHINEDIR}
```

すなわち `${RELEASEDIR}/${RELEASEMACHINEDIR}` ディレクトリがメディアの `${RELEASEMACHINEDIR}` へコピーされる。処理の実態は `src/distrib/common/bootimage/Makefile.bootimage` で記述されている。

```Makefile
.if defined(IMGDIR_EXTRA)
    @echo Copying extra dirs...
.for _SRCDIR _TARGET in ${IMGDIR_EXTRA}
    @if [ ! -d ${_SRCDIR} ]; then                   \
        echo "${_SRCDIR} is not directory, aborting";       \
        false;                          \
    fi
    ${MKDIR} ${MKDIRPERM} ${WORKDIR}/${_TARGET}
    (cd ${_SRCDIR} ;                        \
        ${TOOL_PAX} ${PAX_TIMESTAMP} -rw -pe -v         \
        ${IMGDIR_EXCLUDE}                       \
        . ${.OBJDIR}/${WORKDIR}/${_TARGET} )
.endfor
.endif
```

メディアに追加でなにかを加えたい場合は `${RELEASEDIR}/${RELEASEMACHINEDIR}` にモノを置けばよさそう。
