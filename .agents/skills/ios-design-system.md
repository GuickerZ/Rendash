# Skill: iOS Design System & NativeWind Tokens

Esta skill orienta o desenvolvimento de componentes no aplicativo Rendash seguindo rigorosamente a estética do **iOS Human Interface Guidelines**, adaptada para **React Native + NativeWind (Tailwind CSS)**.

---

## 1. Paleta de Cores e Tokens Tailwind

```javascript
// tailwind.config.js
module.exports = {
  theme: {
    extend: {
      colors: {
        background: {
          primary: '#0B0E14',   // Fundo geral do app
          secondary: '#121824', // Cards e superfícies elevadas
          tertiary: '#1B2234',  // Inputs, divisores e bordas
          elevated: '#222B40',  // Modais e bottom sheets
        },
        accent: {
          emerald: '#00E599',   // Botões primários de sucesso e ação
          teal: '#14B8A6',
          blue: '#3B82F6',
          danger: '#EF4444',
          warning: '#F59E0B',
        },
        text: {
          primary: '#FFFFFF',
          secondary: '#94A3B8',
          muted: '#64748B',
        }
      },
      borderRadius: {
        'ios-sm': '10px',
        'ios-md': '14px',
        'ios-lg': '20px',
        'ios-xl': '28px',
        'ios-full': '9999px',
      }
    }
  }
}
```

---

## 2. Padrões de Componentes Essenciais

### 2.1. Botão Primário Estilo iOS (Pill CTA)
```tsx
import React from 'react';
import { Pressable, Text } from 'react-native';
import * as Haptics from 'expo-haptics';

interface PrimaryButtonProps {
  label: string;
  onPress: () => void;
  variant?: 'emerald' | 'light';
  disabled?: boolean;
}

export function PrimaryButton({ label, onPress, variant = 'emerald', disabled }: PrimaryButtonProps) {
  const handlePress = () => {
    Haptics.impactAsync(Haptics.ImpactFeedbackStyle.Medium);
    onPress();
  };

  const bgStyle = variant === 'emerald' 
    ? 'bg-accent-emerald text-black' 
    : 'bg-white text-black';

  return (
    <Pressable
      onPress={handlePress}
      disabled={disabled}
      className={`h-14 rounded-full flex-row items-center justify-center px-6 active:opacity-85 ${bgStyle} ${disabled ? 'opacity-50' : ''}`}
    >
      <Text className="font-semibold text-base text-black">{label}</Text>
    </Pressable>
  );
}
```

### 2.2. Card com Efeito Dark Glassmorphism
```tsx
import React from 'react';
import { View, ViewProps } from 'react-native';

export function GlassCard({ children, className = '', ...props }: ViewProps) {
  return (
    <View
      className={`bg-background-secondary border border-background-tertiary rounded-3xl p-5 shadow-2xl ${className}`}
      {...props}
    >
      {children}
    </View>
  );
}
```

### 2.3. Switch com Estilo Nativo iOS
- Use `react-native` `Switch` configurado com:
  - `trackColor={{ false: '#1B2234', true: '#00E599' }}`
  - `thumbColor="#FFFFFF"`
  - `ios_backgroundColor="#1B2234"`

### 2.4. Slider de Limite do Cartão com Haptic Feedback
- Utilize `@react-native-community/slider` com estilo escuro e haptic disparado a cada passo significativo de R$ 500.

---

## 3. Estados Visuais Canônicos (Tela 45)

Ao renderizar tags de status, use o componente padronizado:

```tsx
export type VisualStatus = 'PAGO' | 'EM_PROCESSO' | 'BLOQUEADO' | 'CARREGANDO' | 'VAZIO' | 'ERRO';

export function StatusBadge({ status }: { status: VisualStatus }) {
  const configs = {
    PAGO: { bg: 'bg-emerald-500/10', text: 'text-accent-emerald', label: 'PAGO' },
    EM_PROCESSO: { bg: 'bg-blue-500/10', text: 'text-blue-400', label: 'EM PROCESSO' },
    BLOQUEADO: { bg: 'bg-gray-800', text: 'text-gray-400', label: 'BLOQUEADO' },
    CARREGANDO: { bg: 'bg-slate-800', text: 'text-slate-300', label: 'CARREGANDO' },
    VAZIO: { bg: 'bg-slate-900', text: 'text-slate-500', label: 'VAZIO' },
    ERRO: { bg: 'bg-red-500/10', text: 'text-red-400', label: 'ERRO' },
  };

  const current = configs[status];

  return (
    <View className={`px-2.5 py-1 rounded-md ${current.bg}`}>
      <Text className={`text-xs font-semibold uppercase tracking-wider ${current.text}`}>
        {current.label}
      </Text>
    </View>
  );
}
```
