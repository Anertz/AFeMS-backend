# AFeMS-backend 
Minimally configured AFeMS-backend executable files, conf and settings.
> - Ubuntu側のポート開放
> - OCIではサブネットのセキュリティリストの変更も必要
> ```bash
> sudo iptables -I INPUT 5 -p tcp --dport xxxxx -j ACCEPT
> ```
> - 割り込み処理
> ```bash
> sudo systemctl enable irqbalance.service
> ```
> - GraalvmのJITとARMアーキテクチャ用に最適化された起動コード
> ```bash
> ./graalvm-ee-java17-21.3.13/bin/java -Xms18G -Xmx18G -XX:+UnlockExperimentalVMOptions -XX:+UseJVMCICompiler -Dgraal.ProbabilisticProfiling=true -XX:+UseFastUnorderedTimeStamps -XX:MaxInlineLevel=25 -XX:+UnlockDiagnosticVMOptions -XX:+AlwaysActAsServerClassMachine -XX:+AlwaysPreTouch -XX:+EnableJVMCIProduct -XX:+UseSIMDForMemoryOps -XX:+DisableExplicitGC -XX:AllocatePrefetchStyle=1 -Dgraal.OptDuplication=true -Dgraal.SpeculativeGuardMovement=true -Dgraal.Vectorization=true -XX:NmethodSweepActivity=1 -XX:ParallelGCThreads=4 -XX:ConcGCThreads=4 -XX:+UseCriticalJavaThreadPriority -Dgraal.GraalCompileOnly=* -XX:+TieredCompilation -XX:+EagerJVMCI -XX:+ProfileInterpreter -XX:+UseStringDeduplication -XX:CICompilerCount=4 -XX:CompileThreshold=200 -XX:+OptimizeStringConcat -XX:InlineSmallCode=1 -XX:ReservedCodeCacheSize=2048M -XX:ProfiledCodeHeapSize=1024M -XX:NonProfiledCodeHeapSize=512M -XX:NonNMethodCodeHeapSize=512M -XX:-DontCompileHugeMethods -XX:+PerfDisableSharedMem -XX:+EagerJVMCI -Dgraal.TuneInlinerExploration=1 -Dgraal.CompilerConfiguration=enterprise -XX:+UseG1GC -XX:+ParallelRefProcEnabled -XX:MaxGCPauseMillis=100 -XX:G1NewSizePercent=50 -XX:G1MaxNewSizePercent=60 -XX:G1HeapRegionSize=8M -XX:G1ReservePercent=20 -XX:G1HeapWastePercent=5 -XX:G1MixedGCCountTarget=4 -XX:InitiatingHeapOccupancyPercent=15 -XX:G1MixedGCLiveThresholdPercent=90 -XX:G1RSetUpdatingPauseTimePercent=5 -XX:SurvivorRatio=32 -XX:MaxTenuringThreshold=1 -Dusing.aikars.flags=https://mcflags.emc.gs -Daikars.new.flags=true -jar fabric-server-mc.1.20.1-loader.0.16.10-launcher.1.0.1.jar nogui
> ```
> - ネットワークの最適化
> ```bash
> # 4Gbps回線向け、高スループット、低遅延
> # /etc/sysctl.conf
> net.ipv4.tcp_congestion_control = bbr
> net.ipv4.conf.default.rp_filter = 1
> net.ipv4.conf.all.rp_filter = 1
> net.ipv4.tcp_syncookies = 1
> net.ipv4.tcp_fastopen = 3
> net.ipv4.tcp_moderate_rcvbuf = 1
> net.ipv4.tcp_tw_reuse = 1
> net.ipv4.tcp_mtu_probing = 1
> net.core.somaxconn = 65535
> net.core.netdev_max_backlog = 65535
> net.ipv4.tcp_max_syn_backlog = 65535
> net.core.default_qdisc = fq
> net.core.wmem_max = 536870912
> net.core.rmem_max = 536870912
> net.ipv4.tcp_wmem = 4096 1048576 536870912
> net.ipv4.tcp_rmem = 4096 1048576 536870912
> net.ipv4.tcp_no_metrics_save = 1
> net.ipv4.tcp_autocorking = 1
> net.ipv4.tcp_retries2 = 15
> net.ipv4.tcp_retries2 = 15
> net.ipv4.tcp_frto = 2
> net.ipv4.tcp_slow_start_after_idle = 0
> net.ipv4.tcp_retries2 = 15
> net.ipv4.tcp_keepalive_time = 300
> net.ipv4.tcp_keepalive_intvl = 120
> net.ipv4.tcp_keepalive_probes = 15
> ```
