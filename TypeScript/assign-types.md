# Assign Types

## Variables:

```typescript
let age: number = 34
let firstName: string = "Angela"
let busy: boolean = true
let isOpen: boolean
```

For `const` variables, you don't need to assign the type with the assigned value as it would be redundant.

## Functions:

```typescript
function showReviewTotal(value: number, reviewer: string, isLoyalty: boolean) {}
```

## Objects and Arrays:

```typescript
const you: {
    firstName: string;
    lastName: string;
    isReturning: boolean;
    age: number;
    stayedAt: (string | number)[];
} = {
	firstName: 'Bobby',
    lastName: 'Bears',
	isReturning: true,
    age: 23,
    stayedAt: ['florida-home', 'oman-flat', 'tokyo-bungalow', 23]
}

const reviews: {
        name: string;
        stars: number;
        loyaltyUser: boolean;
        date: string;
    }[] = [
    {
        name: 'Sheia',
        stars: 5,
        loyaltyUser: true,
        date: '01-04-2021'
    },
    {
        name: 'Andrzej',
        stars: 3,
        loyaltyUser: false,
        date: '28-03-2021'
    },
    {
        name: 'Omar',
        stars: 4,
        loyaltyUser: true,
        date: '27-03-2021'
    },
]

const properties: {
    image: string;
    title: string;
    perNightPrice: number;
    location: {
        street: string;
        city: string;
        zip: number;
        country: string;    
    };
    contact: string;
    isAvailable: boolean;
}[] = [
    {
        image: 'image.jpg',
        title: 'Bungalow',
        perNightPrice: 200,
        location: {
            street: '100 Main Street',
            city: 'Miami',
            zip: 33156,
            country: 'USA'
        },
        contact: 'annie@me.com',
        isAvailable: true
    }
]
```

