<!-- <template>
  <div class="table-container">
    <el-table :data="tableData" border :hover="false" style="width: fit-content" :scrollbar-always-on="true">
      <el-table-column prop="name" label="系统名" width="180">
        <template #header><div class="header-cell">系统名</div></template>
        <template #default="{ row }">
          <div class="center-cell">{{ row.name }}</div>
        </template>
     / 

      <el-table-column prop="station" label="所属站点" width="180">
        <template #header><div class="header-cell">所属站点</div></template>
        <template #default="{ row }">
          <div class="center-cell">{{ row.station }}</div>
        </template>
     / 

      <el-table-column prop="phoneMan" label="联系人" width="120">
        <template #header><div class="header-cell">联系人</div></template>
        <template #default="{ row }">
          <div class="center-cell">{{ row.phoneMan }}</div>
        </template>
     / 

      <el-table-column prop="map" label="互联网映射" width="300">
        <template #header><div class="header-cell">互联网映射</div></template>
        <template #default="{ row }">
          <div class="center-cell">{{ row.map }}</div>
        </template>
     / 

      <el-table-column prop="serveIp" label="服务地址" width="200">
        <template #header><div class="header-cell">服务地址</div></template>
        <template #default="{ row }">
          <div class="center-cell">{{ row.serveIp }}</div>
        </template>
     / 

      <el-table-column label="调度策略" width="200">
        <template #header><div class="header-cell">调度策略</div></template>
        <template #default="{ row }">
          <div class="split-container" :style="`height: ${getTotalIpLength(row.strategy) * 50}px`">
            <div
              v-for="strat in row.strategy"
              :key="strat.name"
              :style="`height: ${getTotalWafIpLength(strat.waf) * 50}px`"
              class="split-item"
            >
              {{ strat.name }}
            </div>
          </div>
        </template>
     / 

      <el-table-column label="WAF" width="250">
        <template #header><div class="header-cell">WAF</div></template>
        <template #default="{ row }">
          <div class="split-container">
            <template v-for="strat in row.strategy" :key="strat.name">
              <div
                v-for="waf in strat.waf"
                :key="waf.wafName"
                :style="`height: ${waf.ip.length * 50}px`"
                class="split-item"
              >
                {{ waf.wafName }}
              </div>
            </template>
          </div>
        </template>
     / 

      <el-table-column label="节点地址" width="250">
        <template #header><div class="header-cell">节点地址</div></template>
        <template #default="{ row }">
          <div class="split-container">
            <template v-for="strat in row.strategy" :key="strat.name">
              <template v-for="waf in strat.waf" :key="waf.wafName">
                <div v-for="ip in waf.ip" :key="ip" class="split-item fixed-height">
                  {{ ip }}
                </div>
              </template>
            </template>
          </div>
        </template>
     / 

      <el-table-column prop="ssl" label="SSL策略" width="400">
        <template #header><div class="header-cell">SSL策略</div></template>
        <template #default="{ row }">
          <div class="center-cell"><p v-html="row.ssl" style="white-space: pre-wrap"></p></div>
        </template>
     / 

      <el-table-column prop="frontStrategy" label="前置策略" width="300">
        <template #header><div class="header-cell">前置策略</div></template>
        <template #default="{ row }">
          <div class="center-cell"
            ><p v-html="row.frontStrategy" style="white-space: pre-wrap"></p
          ></div>
        </template>
     / 
    </el-table>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const tableData = ref([
  {
    id: 1,
    name: '集团数据底座',
    station: '上海云数据中心',
    phoneMan: '李翔',
    map: '(启用)电信业务TCP:168.1.1.247(80,443)',
    serveIp: '(告警)10.18.21.202:443',
    strategy: [
      {
        name: '汽车供应链',
        waf: [
          {
            wafName: '(正常)10.18.21.249:8443(10)',
            ip: ['(正常)10.18.7.227:80(10)', '(正常)10.18.7.228:80(10)']
          }
        ]
      },
      {
        name: '内部入口',
        waf: [
          {
            wafName: '(告警)10.12.12.91:8416(10)',
            ip: ['(正常)10.18.7.229:80(10)', '(故障)10.18.7.230:80(10)']
          }
        ]
      }
    ],
    ssl: '(卸载)cosocshipping_https(2025/11/13)\n(加密)csntcorp.com(2025/11/13)',
    frontStrategy: '1.HTTP改写\nHTTPS2.汽车供应链\n3.内部入口'
  },
  {
    id: 2,
    name: '物流数据中心',
    station: '北京云数据中心',
    phoneMan: '张伟',
    map: '(启用)物流业务TCP:168.1.2.100(80,443)',
    serveIp: '(正常)10.19.22.150:443',
    strategy: [
      {
        name: '物流追踪',
        waf: [
          {
            wafName: '(正常)10.19.22.248:8443(10)',
            ip: ['(正常)10.19.8.100:80(10)', '(正常)10.19.8.101:80(10)']
          }
        ]
      }
    ],
    ssl: '(卸载)logistics_https(2024/10/15)\n(加密)logistic.com(2024/10/15)',
    frontStrategy: '1.负载均衡\n2.物流追踪\n3.外部访问'
  },
  {
    id: 3,
    name: '金融服务平台',
    station: '深圳云数据中心',
    phoneMan: '王芳',
    map: '(禁用)金融业务TCP:168.1.3.150(80,443)',
    serveIp: '(故障)10.20.23.100:443',
    strategy: [
      {
        name: '支付网关',
        waf: [
          {
            wafName: '(故障)10.20.23.247:8443(10)',
            ip: ['(故障)10.20.9.150:80(10)', '(故障)10.20.9.151:80(10)']
          }
        ]
      }
    ],
    ssl: '(卸载)finance_https(2024/12/20)\n(加密)finance.com(2024/12/20)',
    frontStrategy: '1.安全加密\n2.支付网关\n3.内部访问'
  },
  {
    id: 4,
    name: '人力资源系统',
    station: '广州云数据中心',
    phoneMan: '刘明',
    map: '(启用)HR业务TCP:168.1.4.200(80,443)',
    serveIp: '(正常)10.21.24.150:443',
    strategy: [
      {
        name: '员工门户',
        waf: [
          {
            wafName: '(正常)10.21.24.246:8443(10)',
            ip: ['(正常)10.21.10.200:80(10)', '(正常)10.21.10.201:80(10)']
          }
        ]
      }
    ],
    ssl: '(卸载)hr_https(2025/01/30)\n(加密)hr.com(2025/01/30)',
    frontStrategy: '1.单点登录\n2.员工门户\n3.内外网访问'
  },
  {
    id: 5,
    name: '客户服务中心',
    station: '成都云数据中心',
    phoneMan: '赵静',
    map: '(启用)客服业务TCP:168.1.5.250(80,443)',
    serveIp: '(告警)10.22.25.200:443',
    strategy: [
      {
        name: '客服工单',
        waf: [
          {
            wafName: '(告警)10.22.25.245:8443(10)',
            ip: ['(正常)10.22.11.250:80(10)', '(告警)10.22.11.251:80(10)']
          }
        ]
      }
    ],
    ssl: '(卸载)service_https(2025/03/15)\n(加密)service.com(2025/03/15)',
    frontStrategy: '1.工单分发\n2.客服工单\n3.内部访问'
  },
  {
    id: 6,
    name: '供应链管理',
    station: '武汉云数据中心',
    phoneMan: '孙华',
    map: '(启用)供应链TCP:168.1.6.300(80,443)',
    serveIp: '(正常)10.23.26.250:443',
    strategy: [
      {
        name: '供应商门户',
        waf: [
          {
            wafName: '(正常)10.23.26.244:8443(10)',
            ip: ['(正常)10.23.12.300:80(10)', '(正常)10.23.12.301:80(10)']
          }
        ]
      }
    ],
    ssl: '(卸载)supply_https(2025/04/20)\n(加密)supply.com(2025/04/20)',
    frontStrategy: '1.供应商认证\n2.供应商门户\n3.外部访问'
  },
  {
    id: 7,
    name: '仓储管理系统',
    station: '杭州云数据中心',
    phoneMan: '周强',
    map: '(启用)仓储TCP:168.1.7.350(80,443)',
    serveIp: '(正常)10.24.27.300:443',
    strategy: [
      {
        name: '库存管理',
        waf: [
          {
            wafName: '(正常)10.24.27.243:8443(10)',
            ip: ['(正常)10.24.13.350:80(10)', '(正常)10.24.13.351:80(10)']
          }
        ]
      }
    ],
    ssl: '(卸载)warehouse_https(2025/05/25)\n(加密)warehouse.com(2025/05/25)',
    frontStrategy: '1.库存追踪\n2.库存管理\n3.内部访问'
  },
  {
    id: 8,
    name: '财务管理平台',
    station: '南京云数据中心',
    phoneMan: '吴婷',
    map: '(启用)财务TCP:168.1.8.400(80,443)',
    serveIp: '(正常)10.25.28.350:443',
    strategy: [
      {
        name: '财务报表',
        waf: [
          {
            wafName: '(正常)10.25.28.242:8443(10)',
            ip: ['(正常)10.25.14.400:80(10)', '(正常)10.25.14.401:80(10)']
          }
        ]
      }
    ],
    ssl: '(卸载)finance_https(2025/06/30)\n(加密)finance-mgmt.com(2025/06/30)',
    frontStrategy: '1.报表生成\n2.财务报表\n3.内部访问'
  },
  {
    id: 9,
    name: '研发管理系统',
    station: '西安云数据中心',
    phoneMan: '郑阳',
    map: '(启用)研发TCP:168.1.9.450(80,443)',
    serveIp: '(正常)10.26.29.400:443',
    strategy: [
      {
        name: '代码仓库',
        waf: [
          {
            wafName: '(正常)10.26.29.241:8443(10)',
            ip: ['(正常)10.26.15.450:80(10)', '(正常)10.26.15.451:80(10)']
          }
        ]
      }
    ],
    ssl: '(卸载)rd_https(2025/07/05)\n(加密)rd-system.com(2025/07/05)',
    frontStrategy: '1.代码管理\n2.代码仓库\n3.内部访问'
  },
  {
    id: 10,
    name: '运维监控平台',
    station: '重庆云数据中心',
    phoneMan: '陈亮',
    map: '(启用)运维TCP:168.1.10.500(80,443)',
    serveIp: '(正常)10.27.30.450:443',
    strategy: [
      {
        name: '系统监控',
        waf: [
          {
            wafName: '(正常)10.27.30.240:8443(10)',
            ip: ['(正常)10.27.16.500:80(10)', '(正常)10.27.16.501:80(10)']
          }
        ]
      }
    ],
    ssl: '(卸载)ops_https(2025/08/10)\n(加密)ops-monitor.com(2025/08/10)',
    frontStrategy: '1.监控告警\n2.系统监控\n3.内部访问'
  }
])

const getTotalIpLength = (strategy) => {
  return strategy.reduce((total, strat) => {
    return (
      total +
      strat.waf.reduce((wafTotal, waf) => {
        return wafTotal + waf.ip.length
      }, 0)
    )
  }, 0)
}

const getTotalWafIpLength = (wafArray) => {
  return wafArray.reduce((total, waf) => {
    return total + waf.ip.length
  }, 0)
}
</script>

<style scoped>

.table-container {
  margin:10px;
  margin-top: 100px;
  height: 100%;
  overflow-y: auto;
}

.center-cell {
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.split-container {
  display: flex;
  flex-direction: column;
}

/* 移除cell的内边距 */
:deep(.el-table .cell) {
  padding: 0 !important;
}

.split-container {
  display: flex;
  flex-direction: column;
}

.split-item {
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: #fff;
  border-bottom: 1px solid #000;
  padding: 12px;
}
.fixed-height {
  height: 50px;
}

.split-item:last-child {
  border-bottom: none;
}

.header-cell {
  padding: 12px 0;
  text-align: center;
  font-weight: bold;
}

:deep(.el-table__cell) {
  padding: 0 !important;
}

:deep(.el-table__row:hover) > td {
  background-color: #fff !important;
}

:deep(.el-table__body-wrapper::-webkit-scrollbar) {
  height: 12px;
  width: 12px;
  display: block !important;
}

:deep(.el-table__body-wrapper) {
  /* overflow-x: scroll !important; */
}

/* :deep(.el-table) {
  overflow-x: auto;
}

:deep(.el-table__body-wrapper) {
  overflow-x: auto !important;
}
:deep(.el-table__body-wrapper::-webkit-scrollbar) {
  height: 12px;
  width: 12px;
} */
 

:deep(.el-table--border) {
  border: 1px solid #000;
}

:deep(.el-table--border .el-table__cell) {
  border-right: 1px solid #000;
  border-bottom: 1px solid #000;
}

:deep(.el-table__header-wrapper th.el-table__cell) {
  border-bottom: 1px solid #000;
}

.split-item {
  border-bottom: 1px solid #000;
}
</style> -->

<template>
  <div
    :class="prefixCls"
    class="relative h-[100%] lt-md:px-10px lt-sm:px-10px lt-xl:px-10px lt-xl:px-10px"
  >
    <div class="relative mx-auto h-full flex">
      <div
        :class="`${prefixCls}__left flex-1 bg-gray-500 bg-opacity-20 relative p-30px lt-xl:hidden overflow-x-hidden overflow-y-auto`"
      >
        <!-- 左上角的 logo + 系统标题 -->
        <!-- <div class="relative flex items-center text-white">
          <img alt="" class="mr-10px h-48px w-48px" src="@/assets/imgs/logo.png" />
          <span class="text-20px font-bold">{{ underlineToHump(appStore.getTitle) }}</span>
        </div> -->
        <!-- 左边的背景图 + 欢迎语 -->
        <div class="h-[calc(100%-60px)] flex items-center justify-center">
          <TransitionGroup
            appear
            enter-active-class="animate__animated animate__bounceInLeft"
            tag="div"
          >
            <img key="1" alt="" class="w-350px" src="@/assets/svgs/login-box-bg.svg" />
            <div key="2" class="text-3xl text-white">{{ t('login.welcome') }}</div>
            <div key="3" class="mt-5 text-14px font-normal text-white">
              {{ t('login.message') }}
            </div>
          </TransitionGroup>
        </div>
      </div>
      <div
        class="relative flex-1 p-30px dark:bg-[var(--login-bg-color)] lt-sm:p-10px overflow-x-hidden overflow-y-auto"
      >
        <!-- 右上角的主题、语言选择 -->
        <div
          class="flex items-center justify-between at-2xl:justify-end at-xl:justify-end"
          style="color: var(--el-text-color-primary)"
        >
          <div class="flex items-center at-2xl:hidden at-xl:hidden">
            <!-- <img alt="" class="mr-10px h-48px w-48px" src="@/assets/imgs/logo.png" /> -->
            <!-- <span class="text-20px font-bold">{{ underlineToHump(appStore.getTitle) }}</span> -->
          </div>
          <div class="flex items-center justify-end space-x-10px h-48px">
            <ThemeSwitch />
            <LocaleDropdown />
          </div>
        </div>
        <!-- 右边的登录界面 -->
        <!-- <Transition appear enter-active-class="animate__animated animate__bounceInRight"> -->
        <div
          class="m-auto h-[calc(100%-60px)] w-[100%] flex items-center at-2xl:max-w-500px at-lg:max-w-500px at-md:max-w-500px at-xl:max-w-500px"
        >
          <!-- 账号登录 -->
          <LoginForm class="m-auto h-auto p-20px lt-xl:(rounded-3xl light:bg-white)" />
          <!-- 手机登录 -->
          <!-- <MobileForm class="m-auto h-auto p-20px lt-xl:(rounded-3xl light:bg-white)" /> -->
          <!-- 二维码登录 -->
          <!-- <QrCodeForm class="m-auto h-auto p-20px lt-xl:(rounded-3xl light:bg-white)" /> -->
          <!-- 注册 -->
          <!-- <RegisterForm class="m-auto h-auto p-20px lt-xl:(rounded-3xl light:bg-white)" /> -->
          <!-- 三方登录 -->
          <!-- <SSOLoginVue class="m-auto h-auto p-20px lt-xl:(rounded-3xl light:bg-white)" /> -->
        </div>
        <!-- </Transition> -->
      </div>
    </div>
  </div>
</template>
<script lang="ts" setup>
import { underlineToHump } from '@/utils'

import { useDesign } from '@/hooks/web/useDesign'
import { useAppStore } from '@/store/modules/app'
import { ThemeSwitch } from '@/layout/components/ThemeSwitch'
import { LocaleDropdown } from '@/layout/components/LocaleDropdown'

import { LoginForm, MobileForm, QrCodeForm, RegisterForm, SSOLoginVue } from './components'

defineOptions({ name: 'Login' })

const { t } = useI18n()
const appStore = useAppStore()
const { getPrefixCls } = useDesign()
const prefixCls = getPrefixCls('login')
</script>

<style lang="scss" scoped>
$prefix-cls: #{$namespace}-login;

.#{$prefix-cls} {
  overflow: auto;

  &__left {
    &::before {
      position: absolute;
      top: 0;
      left: 0;
      z-index: -1;
      width: 100%;
      height: 100%;
      background-image: url('@/assets/svgs/login-bg.svg');
      background-position: center;
      background-repeat: no-repeat;
      content: '';
    }
  }
}
</style>

<style lang="scss">
.dark .login-form {
  .el-divider__text {
    background-color: var(--login-bg-color);
  }

  .el-card {
    background-color: var(--login-bg-color);
  }
}
</style>
