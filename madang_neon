
import streamlit as st
import pandas as pd
import psycopg

st.title('Madang 주문 검색')

name = st.text_input('고객 이름')

if st.button('검색'):
    if name.strip():
        sql = '''
            SELECT o.orderid, c.name, b.bookname, o.saleprice, o.orderdate
            FROM Customer c, Orders o, Book b
            WHERE c.name = %s
              AND c.custid = o.custid
              AND b.bookid = o.bookid
        '''
        try:
            conn = psycopg.connect(st.secrets['DATABASE_URL'])
            with conn.cursor() as cursor:
                cursor.execute(sql, (name.strip(),))
                rows = cursor.fetchall()
                columns = [col.name for col in cursor.description]
            conn.close()
            result = pd.DataFrame(rows, columns=columns)
            st.dataframe(result, hide_index=True)
            if result.empty:
                st.write('검색 결과가 없습니다.')
        except Exception as e:
            st.error(str(e))
    else:
        st.write('고객 이름을 입력하세요.')
