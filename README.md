import uuid
import qrcode
from datetime import datetime

class Visitor:
    def __init__(self, name, address, purpose, department):
        self.name = name
        self.address = address
        self.purpose = purpose
        self.department = department
        self.id = uuid.uuid4() # generate unique id for visitor
        self.timestamp = datetime.now() # timestamp of visit

    def generate_qr_code(self):
        qr = qrcode.QRCode(version=1, box_size=10, border=5)
        qr.add_data(self.id)
        qr.make(fit=True)
        img = qr.make_image(fill='black', back_color='white')
        img.save(f"{self.name}.png")

class Logbook:
    def __init__(self):
        self.visitors = []

    def add_visitor(self, visitor):
        self.visitors.append(visitor)

    def generate_receipt(self, visitor):
        receipt = f"Entry Pass\nName: {visitor.name}\nAddress: {visitor.address}\nPurpose: {visitor.purpose}\nDepartment: {visitor.department}\n"
        receipt += f"QR Code: {visitor.id}\n"
        return receipt

def main():
    logbook = Logbook()
    name = input("Enter your name: ")
    address = input("Enter your address: ")
    purpose = input("Enter your purpose: ")
    department = input("Enter the department you will visit: ")
    visitor = Visitor(name, address, purpose, department)
    logbook.add_visitor(visitor)
    receipt = logbook.generate_receipt(visitor)
    print(receipt)
    visitor.generate_qr_code()
    print("QR code saved as {visitor.name}.png")

if __name__ == '__main__':
    main()
