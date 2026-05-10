# EKS Microservices Demo

## Services
- Commerce
- Cart
- Checkout
- Payment

## Deploy

kubectl create namespace commerce

kubectl apply -f commerce/
kubectl apply -f cart/
kubectl apply -f checkout/
kubectl apply -f payment/

## Verify

kubectl get all -n commerce
