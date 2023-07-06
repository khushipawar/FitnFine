import React from 'react';
import { Table, Input, Select } from "antd";

const { Search } = Input;
const { Option } = Select;

interface DataType {
  key: string;
  mapname: string;
  filename: string;
  status: string;
  version: number;
}

const columns = [
  {
    title: 'Map Name',
    dataIndex: 'mapname',
    key: 'mapname',
    filterDropdown: ({ setSelectedKeys, selectedKeys, confirm, clearFilters }) => (
      <div style={{ padding: 8 }}>
        <Search
          placeholder="Search map name"
          value={selectedKeys[0]}
          onChange={(e) => setSelectedKeys(e.target.value ? [e.target.value] : [])}
          onPressEnter={confirm}
          style={{ width: 188, marginBottom: 8, display: 'block' }}
        />
        <Space>
          <Button
            type="primary"
            onClick={confirm}
            icon={<SearchOutlined />}
            size="small"
            style={{ width: 90 }}
          >
            Search
          </Button>
          <Button onClick={clearFilters} size="small" style={{ width: 90 }}>
            Reset
          </Button>
        </Space>
      </div>
    ),
    filterIcon: (filtered: boolean) => (
      <SearchOutlined style={{ color: filtered ? '#1890ff' : undefined }} />
    ),
    onFilter: (value: string, record: DataType) =>
      record.mapname.toLowerCase().includes(value.toLowerCase()),
    onFilterDropdownVisibleChange: (visible: boolean) => {
      if (visible) {
        setTimeout(() => searchInputRef.current?.select(), 100);
      }
    },
    render: (text: string) => text,
  },
  {
    title: 'File Name',
    dataIndex: 'filename',
    key: 'filename',
    width: "50%"
  },
  {
    title: 'Status',
    dataIndex: 'status',
    key: 'status',
    filters: [
      { text: 'Active', value: 'active' },
      { text: 'Building', value: 'building' },
      { text: 'Archived', value: 'archived' },
      { text: 'Inactive', value: 'inactive' },
    ],
    onFilter: (value: string, record: DataType) => record.status === value,
    render: (text: string) => (
      <Select defaultValue={text} style={{ width: 100 }}>
        <Option value="active">Active</Option>
        <Option value="building">Building</Option>
        <Option value="archived">Archived</Option>
        <Option value="inactive">Inactive</Option>
      </Select>
    ),
  },
  {
    title: 'Version',
    dataIndex: 'version',
    key: 'version',
  },
];

interface FileTableProps {
  data: DataType[];
}

const FileTable: React.FC<FileTableProps> = ({ data = [] }) => {
  return (
    <Table columns={columns} dataSource={data} />
  );
};

export default FileTable;
