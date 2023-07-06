# FitnFine

User-focused single page web application for a cross-fit gym.
Conversational and interactive design to engage and intrigue fitness enthusiasts.
Tools and Technologies Used: HTML, CSS, Vanilla JavaScript.




import React, { useState } from 'react';
import { Table, Input, Select } from "anto";

const { Search } = Input;
const { Option } = Select;

interface DataType {
  key: string;
  mapname: string;
  filename: string;
  status: string;
  version: number;
}

interface FileTableProps {
  data: DataType[];
  onSearch: (value: string) => void;
}

const FileTable: React.FC<FileTableProps> = ({ data = [], onSearch }) => {
  const [searchText, setSearchText] = useState("");

  const handleSearch = (value: string) => {
    setSearchText(value);
    onSearch(value);
  };

  const columns = [
    {
      title: 'Map Name',
      dataIndex: 'mapname',
      key: 'mapname',
      filterDropdown: ({ setSelectedKeys, confirm, clearFilters }: any) => (
        <div style={{ padding: 8 }}>
          <Input
            placeholder="Search map name"
            value={searchText}
            onChange={(e) => setSearchText(e.target.value)}
            onPressEnter={() => handleSearch(searchText)}
            style={{ width: 188, marginBottom: 8, display: 'block' }}
          />
          <Space>
            <Button
              type="primary"
              onClick={() => handleSearch(searchText)}
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
      onFilterDropdownVisibleChange: (visible: boolean) => {
        if (visible) {
          setTimeout(() => {
            // @ts-ignore
            searchInput.current.select();
          }, 100);
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

  return (
    <Table columns={columns} dataSource={data} />
  );
};

export default FileTable;
