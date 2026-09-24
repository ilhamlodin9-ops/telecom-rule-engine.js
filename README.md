# telecom-rule-engine.js
JavaScript rule engine for routing telecom customer issues to auto-resolution, information, or human review
const input = $json;

// Generate ONE ticket ID for the entire workflow
const ticket_id = 'TKT-' + Date.now();

// Use Gemini's classification
const intent = input.intent || 'unknown';
const priority = input.priority || 'medium';
const needs_human = input.needs_human ?? true;
const summary = input.summary || 'Customer request requires review';
const confidence = input.confidence ?? 0;

// Map intent to the correct department
const departmentMap = {
  internet_problem: 'Technical Support',
  sim_problem: 'SIM Support',
  recharge_problem: 'Billing',
  call_problem: 'Technical Support',
  sms_problem: 'Technical Support',
  complaint: 'Customer Care / Escalation',
  service_problem: 'Customer Care',
  account_problem: 'Customer Care',
  business_support: 'Corporate Support',
  branch_information: 'Customer Care',
  package_activation_failed: 'Billing',
  unknown: 'Customer Care'
};

const department = departmentMap[intent] || 'Customer Care';

return {
  json: {
    ...input,

    ticket_id,
    intent,
    priority,
    needs_human,
    summary,
    confidence,
    department
  }
};
