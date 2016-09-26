| | **VPN 网关吞吐量 (1)** | **VPN 网关最大 IPsec 隧道数 (2)** | **ExpressRoute 网关吞吐量** | **VPN 网关和 ExpressRoute 共存**|
|--- |----------------------------|-----------------------------------|-------------------------------------|-----------------------------------------|
| **基本 SKU** | 100 Mbps | 10 | 500 Mbps | 否 |
| **标准 SKU** | 100 Mbps | 10 | 1000 Mbps | 是 |
| **高性能 SKU (3)** | 200 Mbps | 30 | 2000 Mbps | 是 |

- (1) VPN 吞吐量是根据同一 Azure 区域 VNet 之间的度量进行的粗略估计。它并不能确保你能够通过整个 Internet 的跨界连接获取多少流量，只能将其用作一种可能的最大度量。
- (2) 隧道数与下面的基于路由的 VPN 相关。基于策略的 VPN 只能支持一个站点到站点 VPN 隧道。
- (3) 基于策略的 VPN 类型不支持高性能 SKU。

<!---HONumber=Mooncake_0822_2016-->