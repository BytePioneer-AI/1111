# Agent Confidence

面向业务 Agent + Skill 的可信等级设计方法与通用设计 Skill。

本仓库帮助开发人员结合真实业务背景、结果结构、验收标准、测试问题和可用证据，定义每项结果的高 / 中 / 低可信条件，并给出适合融入现有 SQL、脚本、API、业务服务或工作流的实施建议。

## 核心边界

本项目**不规定统一运行时架构**：

- 不要求所有业务编写独立校验脚本；
- 不要求统一验证器接口；
- 不要求 `PASS / FAIL / NOT_RUN` 等统一状态；
- 不提供统一置信度聚合器；
- 不要求额外建设置信度服务；
- 不要求可信判断逻辑与现有业务代码分离。

文档和 Skill 负责把业务问题分析清楚、把高 / 中 / 低定义清楚、把证据和审查动作设计清楚。最终由业务团队决定判断逻辑如何落入现有系统。

## 设计重点

1. 明确可信对象：字段、记录、问题答案、测试用例、结果集合或文件；
2. 明确可信含义：高可信具体证明什么，又不证明什么；
3. 明确结果形态和业务阶段；
4. 归纳测试问题并判断是否可泛化；
5. 定义判断维度、硬性规则、高中低条件；
6. 设计证据链、业务影响和人工审查动作；
7. 指定建议实现位置，而不是强制新建框架。

## 三个实际样例

- `examples/pba-so-contract-agent`：SO 合同转换中的字段来源、映射、回退路径和行级追溯；
- `examples/ecs-uat-automation-agent`：规则匹配、单条测试结果正确性、数量与边界覆盖；
- `examples/supplier-questionnaire-agent`：历史 QA 匹配、主体一致性、时效性、来源权威性和部门审查。

## 快速开始

```bash
python skills/agent-confidence-designer/scripts/scaffold_confidence_package.py \
  --agent-id your-agent \
  --agent-name "Your Agent" \
  --output ./confidence-design

python skills/agent-confidence-designer/scripts/validate_confidence_package.py \
  ./confidence-design --strict
```

生成的核心文件：

- `confidence-contract.yaml`：业务范围、结果单元、可信含义、高中低条件和审查策略；
- `known-risk-patterns.yaml`：测试问题、根因、可泛化结论和影响；
- `confidence-logic.yaml`：判断维度、硬性规则、证据要求、建议实现位置；
- `confidence-review-report.md`：设计审查结论、缺口与下一步。

## 验证

```bash
python -m pip install -r requirements.txt
make test
make validate-examples
```
