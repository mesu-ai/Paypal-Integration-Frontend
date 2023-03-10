# Paypal-Integration-Frontend
``
<!-- index.html page -->
		<script defer src="https://www.paypal.com/sdk/js?client-id=<Client-id>&disable-funding=card,paylater&currency=USD"></script>

<!-- PaypalPayment component -->
import axios from 'axios';
import React from 'react';
import ReactDOM from 'react-dom';

const PayPalButton = paypal.Buttons.driver('react', {
	React,
	ReactDOM,
});

const PaypalPayment = ({ packages = {} }) => {
	const style = {
		shape: 'rect',
		color: 'silver',
		layout: 'horizontal',
		label: 'paypal',
		tagline: false,
	};
	const totalPrice = packages?.total;
	console.log(totalPrice);

	const createOrder = (data, actions) => {
		console.log({ data, actions });
		return actions.order.create({
			purchase_units: [
				{
					amount: {
						value: `${totalPrice}`,
					},
					
				},
			],
		});
	};
	const onApprove = async (data, actions) => {
		console.log({ data, actions });
		const res = await axios.get(`https://api-m.sandbox.paypal.com/v2/checkout/orders/${data?.orderID}`, {
			headers: {
				'Content-Type': 'application/json',
				Authorization: `Bearer ${data?.facilitatorAccessToken}`,
			},
		});
		console.log(res);

		return actions.order.capture();
	};

	// useEffect(() => {
	// 	const res = axios.get('https://api-m.sandbox.paypal.com/v2/checkout/orders/', {
	// 		headers: {
	// 			'Content-Type': 'application/json',
	// 			Authorization: 'Bearer Access-Token',
	// 		},
	// 	});
	// 	console.log(res);
	// }, []);

	return (
		<PayPalButton
			style={style}
			createOrder={(data, actions) => createOrder(data, actions)}
			onApprove={(data, actions) => onApprove(data, actions)}
		/>
	);
};

export default PaypalPayment;

``