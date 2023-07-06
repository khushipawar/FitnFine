import React from 'react';
import { Input, Select, Button } from 'antd';

const { Option } = Select;

class SearchField extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      inputValue: '',
      selectedOption: '',
    };
  }

  handleInputChange = (e) => {
    this.setState({ inputValue: e.target.value });
  };

  handleOptionChange = (value) => {
    this.setState({ selectedOption: value });
  };

  handleSearch = () => {
    const { inputValue, selectedOption } = this.state;
    // Perform search logic here
    console.log('Input:', inputValue);
    console.log('Selected Option:', selectedOption);
  };

  render() {
    return (
      <div>
        <Input
          placeholder="Enter text"
          value={this.state.inputValue}
          onChange={this.handleInputChange}
        />
        <Select
          placeholder="Select an option"
          value={this.state.selectedOption}
          onChange={this.handleOptionChange}
        >
          <Option value="option1">Option 1</Option>
          <Option value="option2">Option 2</Option>
          <Option value="option3">Option 3</Option>
        </Select>
        <Button type="primary" onClick={this.handleSearch}>
          Search
        </Button>
      </div>
    );
  }
}

export default SearchField;
