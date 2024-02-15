import React from "react";
import { Icon, Result } from "antd";
import styled from "styled-components";
import { FormComponentProps } from "antd/lib/form";
import Severity from "../../types/enums/Severity";
import { StyledTable } from "../shared/Styled";
import Header from "../shared/Header";
import FinalValidationStatus from "../../types/validation/FinalValidationStatus";

const WarningHeader = styled.h1`
  margin-top: 10px;
`;

interface FinalValidateContainerProps extends FormComponentProps {
  data: FinalValidationStatus[],
  mapName: string,
}
const FinalValidateContainer: React.FC<FinalValidateContainerProps> = (props: FinalValidateContainerProps) => {
  const { data, mapName } = props;
  const errorColumns = [
    {
      title: "Attribute",
      dataIndex: "type",
      key: "type",
      width: 150,
    },
    {
      title: "Status",
      dataIndex: "severity",
      key: "severity",
      width: 100,
      render: (text: string) => {
        switch (text) {
          case (Severity.SUCCESS):
            return <Icon type="check-circle" theme="twoTone" twoToneColor="#52c41a" style={{ fontSize: "24px" }} />;
          case (Severity.WARNING):
            return <Icon type="warning" theme="twoTone" twoToneColor="#f7ba11" style={{ fontSize: "24px" }} />;
          case (Severity.ERROR):
            return <Icon type="close-circle" theme="twoTone" twoToneColor="#ff030b" style={{ fontSize: "24px" }} />;
          default:
            return <Icon type="check-circle" theme="twoTone" twoToneColor="#52c41a" style={{ fontSize: "24px" }} />;
        }
      },
    },
    {
      title: "Message",
      dataIndex: "message",
      key: "message",
    },
  ];
  const warningColumns = [
    {
      title: "Attribute",
      dataIndex: "type",
      key: "type",
    },
    {
      title: "Message",
      dataIndex: "message",
      key: "message",
    },
  ];

  const footer = (type: Severity) => {
    let color;
    let icon;
    let count;
    switch (type) {
      case (Severity.ERROR):
        icon = "close-circle";
        color = "#ff030b";
        count = data.filter((validation) => validation.severity === Severity.ERROR).length;
        break;
      case (Severity.WARNING):
        icon = "warning";
        color = "#f59b42";
        count = data.filter((validation) => validation.severity === Severity.WARNING).length;
        break;
      default:
        break;
    }
    return (
      <>
        {`${count} `}
        <Icon type={icon} theme="twoTone" twoToneColor={color} />
        {` ${type}(s)`}
      </>
    );
  };

  const errors = data.filter((validationResult: FinalValidationStatus) => validationResult.severity === Severity.ERROR);
  const warnings = data.filter((validationResult: FinalValidationStatus) => validationResult.severity === Severity.WARNING);

  return (
    <>
      <Header header="Final Validation Results" fontSize="16px" />
      {errors.length === 0 && warnings.length === 0 && (
      <Result
        status="success"
        title={`Successfully Validated "${mapName}"!`}
        subTitle={"Because your map passed validation, its status has been updated"
        + "from Building to Validated. It is now eligible to be Activated."}
      />
      )}
      {(errors.length > 0) && (
      <>
        {`There were ${errors.length} `}
        <Icon type="close-circle" theme="twoTone" twoToneColor="#ff030b" />
        {` error(s) detected on map ${mapName}. After reviewing, please go to your file 
          and verify the data is as you expect in the highlighted rows and revalidate your map.`}
        <StyledTable columns={errorColumns} dataSource={errors} footer={() => footer(Severity.ERROR)} />
      </>
      )}
      {(warnings.length > 0) && (
      <>
        <WarningHeader>Warnings</WarningHeader>
        <StyledTable columns={warningColumns} dataSource={warnings} footer={() => footer(Severity.WARNING)} />
      </>
      )}
    </>
  );
};




export default FinalValidateContainer;
