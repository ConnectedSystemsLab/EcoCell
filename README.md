EcoCell: Energy-aware Traffic Shaping for Cellular Radio Access Networks
---
---
## EcoCell Highlight
- The rapid growth of cellular networks drives global energy demand: millions of base stations contribute large carbon emissions.
- Traditional approaches for BS energy optimization lack practical, real-world power-saving mechanisms.
- EcoCell introduces a software-only middlebox that reduces energy consumption by shaping traffic patterns in real time.

EcoCell published at NINeS'2026

> **[EcoCell: Energy-aware Traffic Shaping for Cellular Radio Access Networks](https://nines-conference.org/papers/p006-Liu.pdf)**  
> Zikun Liu, Seoyul Oh, Bill Tao, Yaxiong Xie, Anuj Kalia, Deepak Vasisht    
> *NINeS'2026, 2023* 

---
## Dataset

EcoCell Dataset (reproduce figures in the paper): [link](https://uillinoisedu-my.sharepoint.com/:f:/g/personal/zikunliu_illinois_edu/IgDye_Bjwsw_Q7yIrd0eXIkvAYuFAk2wt2XvdGAfo080A1o?e=BMNbSy)
We release all the dataset to reproduce figures in the paper.
We release all the dataset to reproduce figures in the paper, [EcoCell Dataset](https://uillinoisedu-my.sharepoint.com/:f:/g/personal/zikunliu_illinois_edu/IgDye_Bjwsw_Q7yIrd0eXIkvAYuFAk2wt2XvdGAfo080A1o?e=BMNbSy)

Main Datasets Info:

|           | Live Streaming      | Video Streaming     | Web Browsing            | File Downloading    | Video Conferencing    |
| --------- | ------------------- | ------------------- | ----------------------- | ------------------- | --------------------- |
| Single UE | 1_8_live            | 1_21_dash           | 1_7_web                 | 1_8_file            | 1_26_webrtc_3         |
| 5 UEs     | 1_27_live_multi_new | 1_28_dash_multi_new | 1_28_web_multi_new_2000 | 1_29_file_multi_new | 1_29_webrtc_multi_new |

Other Datasets Info:

| Dataset Info                         | Dataset Name                               |
| ------------------------------------ | ------------------------------------------ |
| Ablation Study                       | 1_27_ablation                              |
| Temporal Shifting on 10 trajectories | 1_17_traffic_shifting                      |
| UE Segregation Microbenchmark        | 1_26_toy_fair_1

Usage:
1. Go to individual folder in the dataset link that corresponding to different tasks/experiments (e.g., 1_8_live correpsonding to single UE experiment dataset for live streaming)
2. The individual folder contains:
   1. dataset
   2. src code to analyze and plot the power/DCI information

---

## Traffic Shaping Tool

Short: reads packets from a TUN device, queues them (fifo or per-UE), optionally timestamps for bursty transmission, rate-limits egress, and sends packets via a raw socket.

- Pipeline: `receiver` -> optional `burst_timestamper` -> `scheduler` (`fifo` | `per_ue`) -> `rate_limiter` -> `sender`.
- Bursty mode: sets `earliest_send_time` to group sends into cycles.
- Rate limiting: `rate_limiter` enforces bytes/sec via sleeps and respects timestamps.

Build (Linux, needs root or CAP_NET_RAW/TUN):
```sh
cd src
mkdir -p build && cd build
cmake ..
make -j$(nproc)
```

Run:
```text
./main <input_device> <output_device> <use_bursty> <bursty_cycle> <policy>
```
Example:
```sh
sudo ./main tun0 eth0 true 1.0 per_ue
sudo ./main tun0 eth0 false 0 fifo
```

Note: `main.cc` currently uses fixed values for queue size, quantum and max bytes/sec — edit code or add CLI flags to change them. See `lib/include/scheduler.h`, `lib/include/rate_limiter.h`, `lib/include/sender.h`, and `src/main.cc` for details.

## Citation

If you find this repo and our paper useful, please consider citing our paper:

```bibtex
@inproceedings{LiuEcoCellEC,
   title={EcoCell: Energy Conservation through Traffic Shaping in Cellular Radio Access Networks},
   author={Zikun Liu and Seoyul Oh and Bill Tao and Yaxiong Xie and Anuj Kalia and Usa Deepak OpenAI and Vasisht},
   url={https://api.semanticscholar.org/CorpusID:286090873}
}
```


