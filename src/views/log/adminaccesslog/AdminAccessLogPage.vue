<template>
  <div class="ant-pro-page-container-children-content">
    <!-- 查询条件 -->
    <div class="ant-pro-table-search">
      <a-form v-bind="searchFormLayout">
        <a-row :gutter="16">
          <a-col :md="8" :sm="24">
            <a-form-item label="ID">
              <a-input v-model="queryParam.id" placeholder/>
            </a-form-item>
          </a-col>

          <!-- <template v-if="advanced">
          </template>-->
          <a-col :md="8" :sm="24" class="table-page-search-wrapper">
            <div class="table-page-search-submitButtons">
              <a-button type="primary" @click="reloadTable">查询</a-button>
              <a-button style="margin-left: 8px" @click="resetSearchForm">重置</a-button>
              <!--              <a @click="toggleAdvanced" style="margin-left: 8px">-->
              <!--                {{ advanced ? '收起' : '展开' }}-->
              <!--                <a-icon :type="advanced ? 'up' : 'down'"/>-->
              <!--              </a>-->
            </div>
          </a-col>
        </a-row>
      </a-form>
    </div>

    <a-card :bordered="false" :bodyStyle="{padding: 0}">
      <!-- 操作按钮区域 -->
      <div class="ant-pro-table-toolbar">
        <div class="ant-pro-table-toolbar-title">访问日志</div>
      </div>
      <div class="ant-pro-table-wrapper">
        <!--数据表格-->
        <a-table
          ref="table"
          size="middle"
          :rowKey="rowKey"
          :columns="columns"
          :dataSource="dataSource"
          :pagination="pagination"
          :loading="loading"
          @change="handleTableChange"
        >
          <template #expandedRowRender="record">
            <p>reqParams:</p>
            <pre><div class="wordwrap" v-html="record.reqParams"></div></pre>
            <p>reqBody:</p>
            <pre><div class="wordwrap" v-html="record.reqBody"></div></pre>
            <p>result:</p>
            <pre><div class="wordwrap" v-html="record.result"></div></pre>
            <p>userAgent:</p>
            {{ record.userAgent }}}
          </template>
        </a-table>
      </div>
    </a-card>
  </div>
</template>

<script>
import { getPage } from '@/api/log/adminaccesslog'
import { TablePageMixin } from '@/mixins'

export default {
  name: 'AdminAccessLogPage',
  mixins: [TablePageMixin],
  data () {
    return {
      getPage: getPage,

      columns: [
        {
          title: '追踪ID',
          dataIndex: 'traceId',
          width: '205px'
        },
        {
          title: '用户名',
          dataIndex: 'username'
        },
        {
          title: '请求IP',
          dataIndex: 'ip',
          width: '120px'
        },
        {
          title: '请求URI',
          dataIndex: 'uri',
          ellipsis: true
        },
        {
          title: '方法',
          dataIndex: 'method'
        },
        {
          title: '耗时',
          dataIndex: 'time',
          customRender: function (text) {
            return text + ' ms'
          }
        },
        {
          title: '状态码',
          dataIndex: 'httpStatus'
        },
        {
          title: '错误消息',
          dataIndex: 'errorMsg',
          ellipsis: true
        },
        {
          title: '创建时间',
          dataIndex: 'createTime',
          width: '150px',
          sorter: true
        }
      ]
    }
  },
  methods: {
    onTimeChange (dates, dateStrings) {
      this.queryParam.startTime = dateStrings[0]
      this.queryParam.endTime = dateStrings[1]
    }
  }
}
</script>
