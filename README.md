import React from "react";
import { Form, Input, Icon, Select, Col, Row } from "antd";

const LogicalTransformations = (props) => {
  const {
    index,
    item,
    handleChange,
    handleChangeSelect,
    getDisabled,
    uniqueID,
    transformations,
    editing,
    logicalIndex,
    logicalEnumOptions,
    logicalBoolean,
    logicalTransformBoolean,
    fileColumnProperties,
    checkTargets,
    mappedTargets,
    validations,
    remove,
    getBlankCheck,
    isRestrict,
  } = props;

  const shouldRenderConditions =
    transformations[uniqueID] &&
    transformations[uniqueID].logicalTransformation.length > 1;

  return (
    <div>
      <Col>
        <Select
          allowClear
          showSearch
          dropdownMatchSelectWidth={false}
          style={{ width: 120 }}
          placeholder="AND/OR"
          optionFilterProp="children"
          name={`logicalRelation`}
          onChange={(event) => handleChangeSelect(event, 0, uniqueID, "logicalRelation")}
          value={transformations[uniqueID] && transformations[uniqueID].logicalRelation}
          disabled={!editing}
        >
          <Option value="AND">AND</Option>
          <Option value="OR">OR</Option>
        </Select>

        {validations &&
          validations[uniqueID] &&
          validations[uniqueID].logicalRelation ? (
          <p style={{ color: "red", marginLeft: 15 }}>
            {validations[uniqueID].logicalRelation}
          </p>
        ) : null}
      </Col>

      {shouldRenderConditions && (
        <>
          <Icon
            style={{ marginLeft: 5, marginTop: 10 }}
            className="dynamic-delete-button"
            type="question-circle-o"
            title="AND evaluates all conditions must be true for the overall condition to be true. OR evaluates that one of the conditions must be true for the overall condition to be true."
          />
          {/* Render other elements here when conditions are met */}
        </>
      )}

      {shouldRenderConditions && (
        <>
          <Row style={{ marginBottom: 10 }}>
            <Col span={8}>
              <span>IF</span>

              <Select
                showSearch
                allowClear
                style={{ width: 180 }}
                optionFilterProp="children"
                disabled={
                  getDisabled(item.comparisonFileColumnTargetValueID, item.blank, hardcoded, "targetValue") || !editing
                }
                name={`LogicalTransformation.[${index}].comparisonFileColumnTargetValueID`}
                onChange={(event) => {
                  handleChangeSelect(event, index, uniqueID, "targetValue");
                }}
                value={item.comparisonFileColumnTargetValueID ? item.comparisonFileColumnTargetValueID : fileColumnProperties.logicalTransformation && fileColumnProperties.logicalTransformation[index].comparisonFileColumnTargetValueID}
              >
                {checkTargets(mappedTargets).map((target, i) => (
                  <Option value={target.comparisonFileColumnTargetValueID} key={i}>
                    {/* {target.name}*/}
                    {target.columnName + "-" + target.name}
                  </Option>
                ))}
              </Select>
              {validations &&
                validations[uniqueID] &&
                validations[uniqueID].logicalTransformation &&
                validations[uniqueID].logicalTransformation[index] &&
                validations[uniqueID].logicalTransformation[index].comparisonFileColumnTargetValueID ? (
                <p style={{ color: "red", marginLeft: 15 }}>
                  {validations[uniqueID].logicalTransformation[index].comparisonFileColumnTargetValueID}
                </p>
              ) : null}
            </Col>
            {/* Render other elements here when conditions are met */}
          </Row>
        </>
      )}
    </div>
  );
};

export default LogicalTransformations;
