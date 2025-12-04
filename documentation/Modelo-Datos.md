# Modelo de Datos DynamoDB - Aplicación Financiera

## Tabla: `financial_transactions`

### Schema
```javascript
{
  // Claves primarias
  "pk": "USER#user123",                    // Partition Key (String)
  "sk": "TRANS#2024-01#SALARY_001",        // Sort Key (String)
  
  // Datos de la transacción
  "transactionId": "txn_abc123",
  "type": "income",                        // "income" | "expense"
  "category": "salary",                    // Categoría principal
  "subcategory": "primary_job",            // Opcional
  
  // Información financiera
  "amount": 3000.00,                       // Número (positivo para ingresos)
  "currency": "USD",
  "originalAmount": 3000.00,               // Para conversiones
  
  // Fechas y periodicidad
  "date": "2024-01-15",                    // Fecha específica
  "monthYear": "2024-01",                  // Formato YYYY-MM
  "isRecurring": true,
  "recurringFrequency": "monthly",         // "monthly" | "weekly" | "yearly"
  
  // Método de pago
  "paymentMethod": "bank_transfer",        // "cash" | "credit_card" | "debit" | "bank"
  "creditCardName": "VISA Gold",           // Solo si es tarjeta
  "creditCardLast4": "1234",               // Últimos 4 dígitos
  
  // Descripción
  "description": "Salario mensual",
  "notes": "Pago quincenal",
  
  // Estado
  "status": "completed",                   // "pending" | "completed" | "cancelled"
  "isVerified": true,
  
  // Metadatos
  "createdAt": "2024-01-15T10:30:00Z",
  "updatedAt": "2024-01-15T10:30:00Z",
  
  // Índices secundarios
  "GSI1PK": "USER#user123#CATEGORY#food",  // Para búsqueda por categoría
  "GSI1SK": "2024-01-15",
  
  "GSI2PK": "USER#user123#TYPE#expense",   // Para búsqueda por tipo
  "GSI2SK": "2024-01-15"
}
```

## Tabla: `monthly_summaries`

### Schema
```javascript
{
  // Claves primarias
  "pk": "USER#user123",
  "sk": "SUMMARY#2024-01",
  
  // Totales del mes
  "totalIncome": 5000.00,
  "totalExpenses": 3200.00,
  "netBalance": 1800.00,
  "savingsRate": 36.00,                    // Porcentaje
  
  // Desglose por categoría
  "byCategory": {
    "income": {
      "salary": 4500.00,
      "freelance": 500.00
    },
    "expenses": {
      "housing": 1200.00,
      "food": 800.00,
      "transport": 300.00,
      "entertainment": 150.00,
      "subscriptions": 75.00,
      "credit_card": 500.00,
      "other": 175.00
    }
  },
  
  // Por método de pago
  "byPaymentMethod": {
    "bank_transfer": 1700.00,
    "credit_card": 1200.00,
    "cash": 300.00
  },
  
  // Metas y comparativas
  "budgetGoal": 4000.00,
  "actualSpent": 3200.00,
  "savingsGoal": 1000.00,
  "actualSavings": 1800.00,
  
  // Comparativa mes anterior
  "previousMonthComparison": {
    "incomeChange": 5.2,                   // Porcentaje
    "expensesChange": -2.1,
    "savingsChange": 15.8
  },
  
  // Timestamps
  "updatedAt": "2024-02-01T00:00:00Z",
  "month": 1,
  "year": 2024
}
```

## Tabla: `user_profiles`

### Schema
```javascript
{
  "pk": "USER#user123",
  "sk": "PROFILE",
  
  // Información personal
  "userId": "user123",
  "email": "usuario@gmail.com",
  "name": "Nombre Usuario",
  "avatarUrl": "https://...",
  
  // Configuración financiera
  "defaultCurrency": "USD",
  "monthlyBudget": 4000.00,
  "savingsTarget": 20,                     // Porcentaje del ingreso
  
  // Categorías personalizadas
  "customCategories": [
    {
      "id": "cat_001",
      "name": "Gastos Médicos",
      "type": "expense",
      "color": "#FF6B6B",
      "icon": "medical"
    }
  ],
  
  // Tarjetas de crédito registradas
  "creditCards": [
    {
      "name": "VISA Gold",
      "last4": "1234",
      "dueDay": 15,
      "limit": 5000.00,
      "bank": "Banco XYZ"
    }
  ],
  
  // Cuentas bancarias
  "bankAccounts": [
    {
      "name": "Cuenta Principal",
      "type": "checking",
      "bank": "Banco ABC",
      "last4": "5678"
    }
  ],
  
  // Configuración de notificaciones
  "notifications": {
    "billReminders": true,
    "budgetAlerts": true,
    "weeklyReport": true
  },
  
  "createdAt": "2024-01-01T00:00:00Z",
  "updatedAt": "2024-01-15T14:30:00Z"
}
```

## Global Secondary Indexes (GSI)

### GSI1: Búsqueda por categoría
- **Partition Key:** `GSI1PK` (USER#user123#CATEGORY#{category})
- **Sort Key:** `GSI1SK` (date)
- **Projection:** ALL

### GSI2: Búsqueda por tipo
- **Partition Key:** `GSI2PK` (USER#user123#TYPE#{type})
- **Sort Key:** `GSI2SK` (date)
- **Projection:** ALL

## Tipos de Categorías Predefinidas

```javascript
const CATEGORY_TYPES = {
  INCOME: ["salary", "freelance", "investment", "bonus", "other_income"],
  FIXED_EXPENSES: ["housing", "utilities", "insurance", "subscriptions"],
  VARIABLE_EXPENSES: ["food", "transport", "health", "entertainment"],
  DEBTS: ["credit_card", "loan", "mortgage"],
  SAVINGS: ["emergency_fund", "investment", "retirement"]
};
```